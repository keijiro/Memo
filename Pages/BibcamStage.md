# BibcamStage (Channel 22)

## システム構成

![Schematics](https://user-images.githubusercontent.com/343936/178990585-e5e53546-8cbe-4033-aa50-d5e13e80ff16.png)

- Windows PC (Ryzen 7 3700X, GeForce RTX 2060 SUPER)
- MacBook Pro (M1 Max, Late 2021)
- Novation Launch Control XL
- Komplete Audio 2
- Anker PowerExpand+ 5-in-1 (USB-C ethernet hub)

## プロジェク構成

https://github.com/keijiro/BibcamStage

Unity プロジェクトと Bitwig プロジェクトを含む。

- Bibcam クリップはリポジトリに含まれていない。別途用意して `StreamingAssets` に入れておく必要がある。
- Bitwig に [vzo プラグイン](https://github.com/keijiro/vzo) のインストールが必要。
- Bitwig の標準音源パックが必要。

`BibcamStage` は `BibcamVfx` リポジトリをサブモジュールとして使用する。シーンの基本的なセットアップは `BibcamVfx` の `ThirdPerson` シーンサンプル内に組まれており、 `BibcamStage` はこれをオーバーライド制御するコントローラーとして動作する。具体的には、 UI の表示を消してキーボード操作を加えたり、 vzo 信号を受信して FlashGlitch エフェクトを重ねたり、等々。

こういった作りにしたのは、どうせエフェクトのバリエーションを作るならば、それをそのままサンプルとして流用したい、という思いがあったため。そこで実際には逆に、サンプルを本格的に作り込んで、それをそのままパフォーマンスプロジェクトに流用するという構造にした。

サブモジュールからのアセットの取り込みを円滑化するために、 `BibcamVfx` のアセットはすべて `Packages` ディレクトリに格納されている。 `BibcamStage` プロジェクトからは `file:` 表記を使ってそのパッケージを取り込んでいる。この管理手法は予想以上にうまく機能した。今後も同様の手法を使っていくことができるかもしれない。

## Bitwig

オーディオは基本的に全て Grid システム (Note Grid, Poly Grid, FX Grid) を使って生成している。約 40 トラックほど並べて、ミックスを変えていくことで展開を作る。

シーケンスは基本的に Note Grid を使って生成する。各クリップには全音符のみが置かれていて、ノート・オンの間だけシーケンスを生成し続けるような仕組みになっている。これにより、クリップの停止と共にデバイスがスリープするという連携を作ることができる。

マスタートラックには FX Grid 等を使って構築した汎用ビートリピートを噛ませてある。 Kaoss Pad 的な使い方のできるパフォーマンス・エフェクトであり、 Launch Control に操作をアサインしてある。今回はこのビートリピートと、各トラックのミックスと、各クリップの on/off だけでパフォーマンスを完結した。それ以上のパラメーター操作は行っていない。

