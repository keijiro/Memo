# Swift を使った Unity ネイティブプラグイン開発

SPM (Swift Package Manager) を使って `.dylib` をビルドしていくことになる。

ただし、iOS 向けのビルドを `swift` コマンドで完結させることは難しい。一見できそうに思えるが、実際に試してみると様々な問題が発生する。あまりその用途に使う人がいないために検証されていない……という雰囲気を感じる。

結論として、iOS 向けには `xcodebuild` コマンドを使うことになる。ネットを検索すると `swift` の `generate-xcodeproj` 機能を使う情報が溢れているが、このアプローチは既に deprecated となっている。今は `xcodebuild` コマンドから直接 SPM プロジェクトのビルドが行えるため、その機能を使うことになる。



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
