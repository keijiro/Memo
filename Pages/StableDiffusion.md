# Stable Diffusion

## ハードウェア

Apple Silicon 向けの最適化が進んでおり、Mac 上での運用が実用的な状態となっている。もちろん、最大速度を求めるのであれば CUDA ベースの環境を選択することになるが、Apple Siliocon Mac でも十分に実用的な速度を出すことができる。

昨今の巨大モデルにおける必要メモリ容量の増加を見て、今後は統合メモリアーキテクチャの優位性が伸びると判断し、Apple Silicon を環境として選択することにした。もちろん、運搬が楽である点や、下取りサービスを使った買い替えが楽である点など、一般的なメリットも考慮に入れている。

## Stable Diffusion on Core ML

https://github.com/apple/ml-stable-diffusion

Stable Diffusion を Core ML で動かすためのパッケージ。Apple 製。

Neural Engine に対応しているものの、そのメリットが生きるのは低スペック環境だけであり、強力な GPU を積んでいるデバイスでは普通に GPU を使った方が速い。

### Diffusers

https://github.com/huggingface/swift-coreml-diffusers

HuggingFace によるサンプルアプリ。デフォルトのサンプラーが DPM-Solver++ に変更されている。

汎用性の高いアプリではないが、インストールが手軽であるため（AppStore 経由で入れられる）、デバイスの AI 性能を比較する際のベンチマークとしては役に立つかもしれない。

### セットアップ

1. [ml-stable-diffusion](https://github.com/apple/ml-stable-diffusion) を clone する。
2. [coreml-stable-diffusion-2-base](https://huggingface.co/apple/coreml-stable-diffusion-2-base) を clone する。
3. `ml-stable-diffusion/swift` ディレクトリへ移動。
4. `swift run StableDiffusionSample` でサンプルを実行。

README にはモデルの変換が必要みたいなことが書いてあるが、上記リポジトリには変換済みのファイルが含まれている。

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
- しかしそれでもなお M1 で GPU の強いマシンには勝つことができない。
- 従って現状では M1 の高 GPU スペックマシンを選択するのが望ましいと考えられる。
- M2 において 32 core Neural Engine が実現されたら、状況は変わるかもしれない。
- とりあえず現状 (M2 Max with 16 core NE) において[状況は変わらないようだ](https://github.com/huggingface/swift-coreml-diffusers/issues/31#issuecomment-1459489496)。

## プロンプトエンジニアリング

https://lexica.art/

参考になるようなならないような……以前はプロンプト共有サイトというような体裁であったが、最近は独自モデルを提供しており、汎用的な AI アートサービスとして元を取る形へとピボットしたようだ。
