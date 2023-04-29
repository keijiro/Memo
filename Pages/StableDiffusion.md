# Stable Diffusion

## ハードウェア

Apple Silicon 向けの最適化が進んでおり、Mac 上での運用が実用的な状態となっている。もちろん、最大性能を求めるのであれば CUDA ベースの環境を選択することになるが、Apple Siliocon Mac でも十分に実用的な速度を出すことができる。

昨今の巨大モデルにおける必要メモリ容量の増加を見て、今後は統合メモリアーキテクチャの優位性が伸びると判断し、Apple Silicon を環境として選択することにした。もちろん、運搬が容易である点や、下取りサービスを使った買い替えが楽である点など、一般的なメリットも考慮に入れている。

## Stable Diffusion on Core ML

https://github.com/apple/ml-stable-diffusion

Stable Diffusion を Core ML で動かすためのパッケージ。Apple 製。

Neural Engine に対応しているものの、そのメリットが生きるのは低スペック環境だけであり、強力な GPU を積んでいるデバイスでは普通に GPU を使った方が速い。

### Diffusers

https://github.com/huggingface/swift-coreml-diffusers

HuggingFace によるサンプルアプリ。デフォルトのサンプラーは DPM-Solver++ に変更されている。

汎用性の高いアプリではないが、インストールが手軽であるため（AppStore 経由で入れられる）、デバイスの AI 性能を比較する際のベンチマークとして役に立つ。

### サンプルを動かすまで

変換済みの Stable Diffusion モデルを使用することで簡単に試すことができる。

1. [ml-stable-diffusion](https://github.com/apple/ml-stable-diffusion) を clone する。
2. [coreml-stable-diffusion-2-base](https://huggingface.co/apple/coreml-stable-diffusion-2-base) を clone する。
3. `ml-stable-diffusion/swift` ディレクトリへ移動。
4. `swift run StableDiffusionSample` でサンプルを実行。

### ベンチマーク

- Model: Stable Diffusion v2 (base)
- Guidance scale: 7.5
- Step count: 10

|               | M1 Max MBP | M2 MBA |
| ------------- | ---------- | ------ |
| GPU           |       3.3s |   8.5s |
| Neural Engine |       6.1s |   4.4s |
| GPU + NE      |       5.6s |   5.0s |

- M2 における Neural Engine の強化は確実に効果があり、M2 では NE を使用した場合にかなりの高パフォーマンスが期待できる。
- しかしそれでもなお GPU の強いマシンには勝つことができない。
- とりあえず現状では GPU だけを性能評価基準軸として見ておけばよい。

### モデル変換

基本的には "Converting Models to Core ML" にある通りの手順でコンバートできる。

個人的に Anaconda の使用は好まないが、`conda` を使わずに変換することは難しい。大人しくインストールせざるを得ない。

"Original" モデルの `.mlmodelc` ファイル群を得るために発行するコマンドは下記のようになる。

```
python -m python_coreml_stable_diffusion.torch2coreml --attention-implementation ORIGINAL\
  --chunk-unet --convert-unet --convert-text-encoder --convert-vae-decoder --convert-vae-encoder\
  --model-version stabilityai/stable-diffusion-2-base --bundle-resources-for-swift-cli -o models
```

### その他注意点

- 0.4.0 以降に導入された高速化処理の影響でモデルファイルの互換性が断絶している。Text To Image であれば実行が可能であるが、 Image To Image では例外が発生する。これに対処するには、0.4.0 以降のバージョンの torch2coreml を使ってモデルを再変換する必要がある。一応 [issue は投げた](https://github.com/apple/ml-stable-diffusion/issues/176)が、正直な所、誰がどう対処すべきなのかよく分からない。単純に公開されているコンパイル済みモデルを一通り再コンパイルして再公開するのが最適解か？

## プロンプトエンジニアリング

https://lexica.art/

参考になるようなならないような……以前はプロンプト共有サイトというような体裁であったが、最近は独自モデルを提供しており、汎用的な AI アートサービスとして元を取る形へとピボットしたようだ。
