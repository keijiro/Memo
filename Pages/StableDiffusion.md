# Stable Diffusion

しばらくしたら小慣れて使いやすくなってくるだろう……と思い、しばらくしてから覗いてみたら、想像以上に混沌としていた。



## 戦略

### 妥協 Mac コース

Apple Silicon MacBook 上で SD を動かす。 Apple による最適化のおかげで、そこそこのパフォーマンスを出してくれるが、それでも限界があるので、品質的にはかなりの妥協を行う必要がある。ただし、機材の運用は非常に楽であり、現場での運用には適している。

### そこそこ PC コース

Mini-ITX PC 上で SD を動かす。使用できるグラボの大きさと消費電力に限界があるため、パフォーマンスにはある程度の妥協が生じる。総合的にみてコスパは最も悪いかもしれない。

### 本気 PC コース

ATX タワー PC を導入する。パフォーマンス的には最も高いものが実現できるが、相応の予算が必要になるし運搬も大変になるため、あまりやりたくない。

### クラウドコース

GPU インスタンスを借りて SD を動かす。最も合理的な選択であるが、現場での運用に難がある。まともに使える高速なネット回線を用意できる現場というのは案外に少ない。



## Stable Diffusion on Core ML

https://github.com/apple/ml-stable-diffusion

Stable Diffusion を Core ML で動かすためのパッケージ。Apple 製。

Neural Engine 対応で超高速……みたいな雰囲気を出しているが、実際には Neural Engine の効果はそれほど高くない。強力な GPU を積んでいる場合は普通に GPU を使った方がむしろ速い。モバイルデバイスで動かす場合には有利か。

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
