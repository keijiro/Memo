# WSL 上で Windows 向けにクロスコンパイル

まず Rust のインストール。公式アプローチで。

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

`x86_64-pc-windows-gnu` ターゲットと mingw のインストールを行う。

```
rustup target add x86_64-pc-windows-gnu
sudo apt install gcc-mingw-w64-x86-64
```

あとは好きなプロジェクトを `x86_64-pc-windows-gnu` ターゲットでビルドするだけで良い。

```
cargo build --target=x86_64-pc-windows-gnu
```

# Unity のネイティブプラグインに使う

実装のパターンを UnityRustTestbed プロジェクトに集約してある。

https://github.com/keijiro/UnityRustTestbed

注意点としては、 crate-type として `dylib` ではなく `cdylib` を用いること。 `dylib` は Rust ネイティブの ABI であるため C との相互運用ができない。

## 関数シグネチャ

マングリングを避けるために `#[no_mangle]` を与えること。また、システム ABI を用いるために `extern "C"` で修飾する必要があるとされている（多くの場合これを付けなくても通ってしまうが）。

## 構造体レイアウト

https://doc.rust-lang.org/reference/type-layout.html#representations

デフォルトのレイアウトは不定。勝手に並べ替えが行われる。 C# Interop などで使うには `#[repr(C)]` が必須。

## Pros/Cons

基本ライブラリのフットプリントが大きいため、小規模なプラグインではデメリットが目立つ。中規模以上のプラグインであれば気にならない。

プラグイン開発に Rust を使用するメリットは、 Rust 言語自体の存在よりも、 Cargo を中心としたエコシステムの方にあると考えられる。 Cargo によって実現されているビルドシステムとパッケージ管理は、 C/C++ ベースの開発では絶対に手に入らないものであり、そこに決定的な価値があると感じた。

ゆえに、実際に Rust を使うかどうかという判断は、目的とする機能が Cargo 経由で入手可能かどうかによって行われることになる。もし Cargo でパッケージが簡単に入手可能なら Rust を使う価値がある。

ここで問題になるのは、 Rust のパッケージレジストリはまだまだ発展途上であるということ。 crates.io 上には無数のパッケージが存在するが、継続してメンテナンスされているパッケージは案外少ない。よりどりみどりに見えて、実はそんなに使えないというのが現実。

ただ Rust の流行に合わせて発展のペースは上がってきているはずなので、今後の推移を見ながら適宜判断していくのが良いかもしれない。