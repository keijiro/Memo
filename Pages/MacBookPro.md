# M2 Max MacBook Pro (2023) vs M4 Max MacBook Pro (2024)

- M2 Max MacBook Pro, 16-inch 2023, 64GB RAM, 12 CPU Cores, 38 GPU Cores
- M4 Max MacBook Pro, 16-inch 2024, 64GB RAM, 16 CPU Cores, 40 GPU Cores

## NoiseBall6 - Frame Rate (3,276,000 triangles)

- M2 Max: 155 fps
- M4 Max: 205 fps

## NoiseBall6 - Web Build Time

- M2 Max: 3 min 23 sec
- M4 Max: 2 min 53 sec

## Fantasy Kingdom - Reimport All

- M2 Max: 1 min 35 sec
- M4 Max: 1 min 27 sec

## Fantasy Kingdom - Frame Rate

- M2 Max: 240 fps
- M4 Max: 270 fps

## DaVinci Resolve 4K H265 Rendering

- M2 Max: 8 min 50 sec
- M4 Max: 6 min 45 sec

## Stable Diffusion 1.4 (9 steps)

- M2 Max: 2.5 sec
- M4 Max: 1.5 sec

## sd-turbo (2 steps)

- M2 Max: 0.8 sec
- M4 Max: 0.5 sec

# M2 Max MacBook Pro (2023) を導入すべきかどうか問題

## 前提

- 普通に考えればアップデートする動機はあまり無い。
- しかし、機械学習モデルを高速に動かしたいという欲求が急速に高まっており、現状で最高スペックを持つ M2 Max MBP を導入する意欲が高まっている。
- つまり、以前の機種と比較して機械学習モデルが高速に動くかどうか、と言う点だけが判断の基準となる。

## 14" か 16" か

- 通常のベンチマークでは性能にほぼ差が無い。
- しかし極限まで GPU を酷使するベンチマークでは差が出る。 10% 程度出ることも。
- 価格差はそこまで大きくない（３〜４万円程度）

https://www.youtube.com/watch?v=wakmaa5_GQk&t=152s

最高スペック構成で 3D ベンチマークで 20% 程度の差が出ている。

結論： 16" を選択すべき

## Neural Engine は考慮に入れるべきか

現状、Neural Engine のメリットが現れるのは低スペックマシンの場合のみであり、高スペック環境では GPU の動作が支配的となる。Neural Engine のスペックは考慮から完全に外して構わない。

## M1 Max MBP との性能比較

### Hugging Face Diffusers

- [Hugging Face のブログ記事](https://huggingface.co/blog/fast-mac-diffusers)によれば、 M1 Max の最高スペックで 8.3 秒。
- [ユーザーからの報告によれば](https://github.com/huggingface/swift-coreml-diffusers/issues/31)、 M2 Max の MBP 14" 最高スペックで GPU 推論が 6.5 秒。
- 現在使用している M1 Max 16" の最高スペックで 7.5 秒。

Hugging Face とウチで 0.8 秒も違いがあるのが謎だが、恐らく Hugging Face の報告は 14" で計測したものではないかと思われる。それならば 10% 近くの差があることに納得がいく。もし M2 Max でも同様に 10% 程度の違いが出るとすれば、 M2 Max 16" では 5.9 秒程度にまで速くなることが期待できるか？

# M1 Max MacBook Pro (2021)

## ProMotion

ProMotion を有効にしている場合、 Unity Editor および Standalone Player は 120 fps ベースで動く。 60 fps で動かしたい場合は Target Frame Rate の指定を行うか、 ProMotion を無効にする必要がある。
