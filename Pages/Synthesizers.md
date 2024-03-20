# Synthesizers

今までに使ってきたシンセについて。

## Roland MC-101

いわゆるグルーヴボックスだが、豊富なプリセット音色を使ってリッチな音作りを楽しむ、という方向性に振り切っており、その点で他の製品と決定的な差別化を図ることができている。またそのため、VA シンセやサンプラー音源のような自力での音作りに拘りたいたいと考えている場合は期待外れになるかもしれない。その拘りが無ければ、他のグルーヴボックスとは異なる体験を楽しむことができると思うし、個人的にはそこに大きな楽しみを見つけることができた。

音源には Zen Core を使用しており、出音の厚さは素晴らしい。プリセットも非常に充実している。ただ、リズム音色は正直かなり弱い。定番の 808/909 等は使えるが、それ以外の音色についてカバー範囲がかなり狭いと感じる。自前でのサンプルの用意が必須になるかもしれない。

## Dirty Wave M8 Tracker

https://dirtywave.com

最近まで、これは LSDj に代表されるような、懐古主義路線の古典的トラッカーだと思っていた。しかし、ひょんな事からこの製品を調べてみたところ、とんでもない化け物である事が分かってきた。

### M8 Tracker とは

体裁としてはいわゆるトラッカーだが、サンプラーを基本とした古典的なトラッカーとは異なり、多彩な音源とエフェクターを搭載している。各トラックにフィルター＆リミッターを搭載し、センドエフェクトとしてディレイとリバーブも搭載しているため、トラッカーらしくない空間的広がりのある音作りが可能。和音に対応した音源も存在しており、編曲の幅は非常に広い。

また、複雑なシーケンスエフェクトを搭載しており、シーケンスの面でもかなり変わったことができる。総じて工夫の余地が非常に広いシンセである。

また、個々の音源の出音が非常に良かったり、制御面でも細かな配慮が行き届いており（ソングをシームレスに切り替えられるのは感動しかない！）、スペック表では語られない部分の「設計の良さ」が極まっている感がある。

### M8 Headless とは

……と M8 Tracker を褒めまくったものの、実はまだ M8 Tracker 実機を手にしたことは無い（！）

なら何故 M8 を使っているかのように語れるかというと、M8 Headless をベースとした互換環境が存在するからである。

M8 Headless というのは、有り体に言えば M8 Tracker のシーケンサ＆音源部分を独立させて、入出力部分を持たない所謂 "Headless" として実装したもの。 Teensy というマイコンボードの上で動かす事ができる。どうやら M8 Tracker 自体が内部で Teensy を使用して実装しているらしく、その部分を独立して動かせるよう公開したもの、と考えることができる。

元はこの Headless を PC 側クライアントと接続してライブパフォーマンスなどで使えるようにするのを狙ったものらしい。だが今は、もっぱら M8 Tracker を入手できない人が仮に使うための環境として普及しているようだ（M8 Tracker は生産数が非常に少ないため入手困難状態が続いている）