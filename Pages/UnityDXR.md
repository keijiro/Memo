# Unity DXR (realtime ray tracing)

## 対応環境

- DXR on DX12

公開プラットフォームではこれだけ。 PS 5 / Xbox X|S も対応しているらしいが NDA 下のため公開情報無し。

## できること

### スクリーンスペース系のポストエフェクトのレイトレ化

Screen-Space Ambient Occlusion, Screen-Space Raytraced Reflection や Screen-Space Global Illumination をレイトレース化することができる。名前に "Screen-Space" と入っちゃってるのにシーン空間レイトレースになるのは違和感あるが、仕組みとしては扱いやすい。

### Path tracing 対応

Unity のシーンをパストレでレンダリングすることができる。単純にこれはけっこうすごい。 OptiX や Open Image Denoise を使ったノイズ除去にも対応しており、かなり実用的なプリレンダーワークフローを組むことができる。

Refraction や DoF について特殊処理が走るようになっており、屈折やレンズボケに関してリアルタイムとは異なるレベルのビジュアルを実現できる。
