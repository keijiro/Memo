# macOS

`.dylib` のライブラリは拡張子を `.bundle` に置換することでそのまま使用できる。

## Universal binary の作り方

1. `aarch64-apple-darwin` で `.dylib` を生成
2. `x86_64-apple-darwin` で `.dylib` を生成
3. `lipo` を使って合成

```
lipo -create -output test.bundle test_arm.dylib test_x86.dylib
```

## gcc でフレームワークをリンクする

`-framework` オプションを使う。

`gcc test.c -framework Foundation`

# iOS

`.m` や `.c` などのソースファイルを置くことで対応もできるが、ファイル数が多くなって煩雑。 `.a` の形で取り込むのが良い。

`Info.plist` の設定等はビルドスクリプトで自動対応できる。[対応例](https://github.com/keijiro/KlakNDI/blob/main/jp.keijiro.klak.ndi/Editor/PbxModifier.cs)。

# Apple silicon

`.bundle` を入れ替え後、例外が発生するようになることがある（原因不明）。下記の手順で回避できる模様。

- Editor を落としてから `.bundle` および `.bundle.meta` を一旦削除し、コピーし直してから Editor を起動。
- 上記の手順に加えて、一旦 Hub を落とすところまでやる。