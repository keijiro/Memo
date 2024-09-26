# Dcam

Stable Diffusion ベースの映像システムを構築し Channel #23 で使用した。

## 概要

[Apple が開発している Stable Diffusion の Core ML 移植版](https://github.com/apple/ml-stable-diffusion) を使用し、カメラから入力された映像をセミリアルタイムに image-to-image 変換するシステムを構築した。

この image-to-image パイプラインでは、１フレームの変換に最低でも 1.5 秒ほどかかる。この待ち時間を如何に隠すか、というのが課題となる。

今回は flip book 風のアニメーションで間を繋ぎ、この待ち時間を感じさせない工夫とした。

## 取り組んだ課題と解決法

### ハードウェアの選択

**課題** - Stable Diffusion を動作させるための PC を一人で運搬・運用するのは困難。

**解決** - Apple の Core ML Stable Diffusion を導入することで MacBook Pro １台での運用が可能になった。

### プラグインの開発

**課題** - Core ML Stable Diffusion は Swift で書かれており、ノウハウの少ない Swift でのネイティブプラグイン実装を行う必要がある。

**解決** - [簡単なプロジェクト](https://github.com/keijiro/UnitySwiftPluginTest)から始め、ノウハウを蓄積した。

### Stable Diffusion の速度

**課題** - Stable Diffusion の image-to-image 変換には最低でも 1.5 秒かかる。

**解決** - Flip book アニメーションを使ってこれを表現の中に組み込んだ。

### カメラ入力

**課題** - カメラから映像をいかに入力するか。また、カメラ操作は誰が行うか？

**解決** - NDI を使用してリモート操作を可能にした。撮影と操作を iPhone から遠隔で行うことができる。

## 改良

ml-stable-diffusion に sd-turbo (LCM) 対応の追加を試みている人が複数おり、これらの fork を参考にして、[sd-turbo 対応を実現できた](https://github.com/keijiro/ml-stable-diffusion)。

sd-turbo の導入により、推論の step 数を２にまで減らすことが可能になった。実際には step=2 では制御の難しい部分があり、step=4 での運用が基本になる。

Step=2 での推論は 0.8 秒程度、step=4 では 1.1 秒程度に収まる。

sd-turbo の欠点は strength の制御がほぼ効かなくなるという点。これについては、元々 strength 制御はそれほど使っていなかったと割り切ることにした。Strength=50% での運用が基本になる。Step=4 ならオプションとして Strength=75% も選択できるが、かなり大幅に絵を変えてしまうので、エフェクトとしては使いにくくなる。
