# NeRF

NeRF ってどうなんだろう。

## VFX 的な使用方法

https://www.youtube.com/watch?v=YX5AoaWrowY

テンション上がる動画。とりあえずこれを観てモチベ上げる。

## NeRF の要点

- Radiance Field ということで反射などの複雑な表現も再現できるのが旨味であり、それが活かせないなら使う意味はあまり無い（かもしれない）。
- Photogrammetry と比べてメリットはあるのか？ という気持ちが常にある。↑が唯一確かなアドバンテージである。それが活かせないなら Photogrammetry で良いのでは？ となる。
- ストレージやメモリの消費量は激しい。リソース節約の手段として活きる場面はあまり無さそう。
- ツール類はまだあまり整備されていない。当初と比べればボチボチ出てきている方だが……

## ツール

### Luma AI

https://lumalabs.ai

なんか良さげだが情報が非公開過ぎる。自前で応用するのは難しそう。

### Nerfstudio

https://github.com/nerfstudio-project/nerfstudio

オープンソースのツールセット。とりあえずこれを使うのが良さそう。

