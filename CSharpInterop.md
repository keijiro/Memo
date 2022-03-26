# C# Interoperability

C# の managed code と unmanaged code の連携について。ここで扱うのは Unity 上における Platform Invoke だけであり、 COM Invoke 等は考慮しない。

## 方針

### Blittable 型のみで済むケース

Interoperability におけるデータのやり取りを blittable 型のみで完結させるというのは、一つの理想系。マーシャリングを行わずに interoperability を実現することができる。

関数の引数には参照や配列を使っても構わない。 Unmanaged 側ではポインタとして受け取ることができる。これらは pinning によって unmanaged 側に引き渡されるため、余計なコピーは発生しない（はず）。

### マーシャリングを用いるケース

Unmanaged 構造体がポインタを含むようなケースや、 `bool` のような非 blittable 型が多用されるケース、文字列のやり取りが発生するケースなどでは、マーシャリングを用いた方がコードが簡潔にまとめられる可能性がある。そのような場合はマーシャリングを活用する。

ただ、マーシャリングとの相性はそのライブラリのメモリ管理の方針によって変化する部分があり、本当にマーシャリングを用いるべきかどうかはケースバイケースで判断する必要がある。マーシャリングとの相性が悪いと感じたら、 早々に生ポインタないしは `IntPtr` を用いた「自前マーシャリング」に切り替えるべきかもしれない。

### In/Out 属性の付与

関数の引数に参照／配列を用いる場合は明示的に `In`, `Out` 属性を与えること。この属性付けに実質的な効果があるとは限らないが、可読性の面から付けておいた方が良い。ただし `out` 修飾子を用いる場合は不要（`Out` 属性であることは自明なので）。

### 配列の扱い

https://docs.microsoft.com/en-us/dotnet/framework/interop/default-marshaling-for-arrays

P/Invoke で配列を渡す場合のデフォルトの挙動は `[In]` 属性になるらしい。ただ、 blittable 型の配列を渡した場合、 pinning によって実メモリブロックが渡されるため、 `[In]` 属性でも書き換えができてしまう模様。

非 blittable 型の配列は大量のマーシャリングが発生することになるため、できるだけ使用は避けたい。

### 文字列の扱い

https://docs.microsoft.com/en-us/dotnet/framework/interop/default-marshaling-for-strings#strings-used-in-platform-invoke

引数として managed から unmanaged への引き渡しを行う場合、マーシャリングを用いるのが無難。デフォルトでは `[MarshalAs(UnmanagedType.LPStr)]` となり、つまりヌル終端 ANSI 文字列へのポインタとして渡されることになる。なお、この文字列は読み取り専用となり、書き換えることはできない。

Unmanaged 側で文字列の書き換えを行いたい場合は `StringBuilder` を用いる。 C# 側のシグネチャでは `StringBuilder` を引数とし、 unmanaged 側では `char*` として受け取る。バッファサイズは `Capacity+1` を渡す。

Unmanaged から返値として文字列を返すケースは、メモリの扱いが問題になる。返されたメモリブロックの所有権は managed 側に渡り `CoTaskMemFree` で解放されることになるので、つまり unmanaged 側では `CoTaskMemAlloc` を使ってメモリを確保する必要があるが、これは Windows 以外ではどうしたら良いのか？ 多くの場合は malloc を使っておけばよいようだが、例えば WebGL ではどうなる？ などと考え始めると難しい。文字列返値をどうしても使用したい場合は、メモリ管理は自前で行い、受け渡しには `IntPtr` を用いるようにするのがよいだろう。

## Marshaling

https://docs.microsoft.com/en-us/dotnet/framework/interop/interop-marshaling

### 返値のマーシャリング

`[return: MarshalAs(..)]` のようにする。

例えば、１バイト bool 値を受け取る場合：

`[return: MarshalAs(UnmanagedType.U1)] static extern bool Foo();`

### Struct Layout

`struct` における `StructLayout` のデフォルト設定は `Sequential` なので、これを明示的に付与する必要は無い。

### Blittable / Non-blittable

- C# の `enum` は表現型の一種で、実体には任意の整数型を使うことができる。つまり blittable である。
  - デフォルトは `int` が使用される。
  - `enum Foo : ushort` のようにしてデータ型を変更できる。
- `bool` は blittable ではない。
  - Interop で使用するには、 `MarshalAs` を使ってマーシャリングするか、 `uchar` などの型で置き換える必要がある。

## SafeHandle

生存期間の概念が存在する unmanaged オブジェクトを扱うには `SafeHandle` を使うのが便利。継承によってカスタマイズ可能なスマートポインタのようなもの。あたかも `IDisposable` を実装した managed クラスのように扱える。また、マーシャリングによって `void*` と `SafeHandle` の間の自動変換が行われるため、仲介コードの実装も大いに簡略化される。

https://blog.benoitblanchon.fr/safehandle/

KlakNDI 等で多用しているので、それをベースにして使用する。[この辺り](https://github.com/keijiro/KlakNDI/blob/main/jp.keijiro.klak.ndi/Runtime/Interop/Recv.cs)の実装を参照のこと。

- コンストラクタは private で、 static ファクトリ関数を用意する。
- `ReleaseHandle` を実装する。
- P/Invoke による unmanaged エントリポイントは private に隠す。
- それらに対応する public 関数を用意する。

## 細々としたこと

### `bool` に `MarshalAs` は必要か？

`struct` のメンバーには必須。関数の引数／返値については、データサイズの差異がスタック上のアラインメントで吸収されるため、 `MarshalAs` を与えなくとも結局は正常に動いてしまう。