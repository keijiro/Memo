# 独自プレゼンテーションシステム

MacBook 上に動画を流しつつ iPhone 上に台本を表示するシステム。

## Scrubber

https://github.com/keijiro/Scrubber

動画のプレイバックを基本としたプレゼンテーションシステム。HAP コーデックを使用することで時間軸の自由な操作を可能にしている。これによって動画の再生位置と喋りの速度を合わせることができるようになり、事前録画・編集された動画をあたかもライブデモのように見せることが可能になる。オンスクリーン描画機能を使った注釈の書き込みなどにも対応している。

## PromptLink

https://github.com/keijiro/PromptLink

NDI を使った台本表示システム。台本テキストをオフスクリーンレンダリングしつつ NDI で iPhone に送信する。

正式なパッケージ化は行っていない。U/Day Tokyo ではプレゼンテーション用プロジェクトにコピペで組み込む形にした。

## 実際のプロジェクト

https://github.com/keijiro/UITK2024Deck

2024 年の U/Day や Unite で使用した UI Toolkit 講演用のプロジェクト。動画ファイルはサイズが非常に大きいためリポジトリからは除外してある。

## 拡張案

### 特殊な入力装置の採用

理想を言うならば、動画の再生位置操作にはロータリーエンコーダーを使用したい。過去には Kensington のトラックボールに付いているリング状ホイールを使用したこともあった。

https://www.kensington.com/p/products/electronic-control-solutions/trackball-products/orbit-trackball-with-scroll-ring/

操作感は非常に良好だった。ただし、このような特殊な装置に依存するということは荷物を増やすことに繋がるし、まだ装置の喪失（故障や紛失、忘れ物等を含む）のリスクを抱えることになる。

2024 年の講演ではこれをトラックパッドに変更してみたが、使用感はそこまで大きく変わらなかった。上記のようなリスク要素を増やすまでではなかろう、というのが現状の結論になる。

### 動画再生ハードウェアの使用

動画再生の仕組みは Blackmagic の Hyperdeck Shuttle HD で代替できるかもしれない。

https://www.blackmagicdesign.com/products/hyperdeckshuttlehd

これは大型のジョグダイアルを備えたスタンドアロン型の録画・再生装置であり、Scrubber の機能をこれで実現できる可能性がある。また、プロンプター機能も内蔵している。

ただし、注釈の書き込みなどには対応していないし、映像出力は１系統しか持っていないため、プロンプターを活用するには２台用意する必要がある。

可能性は感じられるが、現状においては代替物たりえないという結論。また、急に不具合が生じた場合に代替を用意しにくいという弱点もある。

## 実演時のトラブルの記録

### U/Day Tokyo (2024/7/1)

本番前のステージ上でのセットアップ時に iPhone 側の NDI モニタ (Nsm) がなかなか NDI ホストを認識しないというトラブルが発生した。MacBook のリブート後に解決。原因はよく分からないが、このようなトラブルを避けるためにも、ステージへ上がる前から接続を確立しておき、その接続を維持したままステージに上がるという順序にした方が良いだろう。

また、講演中にバッテリーが切れそうになる（Low Power 警告が表示される）というトラブルも発生した。これは単純にデバイス管理の不手際であり、一般的な対処で解決可能。

### Unite Shanghai (2024/7/25)

ミラーリングされた画面との同期がうまくいかないという不具合が発生した。具体的には、プロジェクター側の外部出力のみが画面更新され、本体の内蔵ディスプレイが更新されない（表示がフリーズする）という状態になった。不幸中の幸いとして外部出力の方は問題無く生きていたため、返しのモニターやプロジェクター画面を見れば問題なく操作する事ができた。正直なところ、それよりも表示遅延の方がしんどかった（再生ポイントがオーバーランしてしまいがちになるため）

なぜミラーリングの同期が外れたのか、その原因については全く不明。接続後にアプリ起動したため、接続順の問題ではないと思われる。USB-C to HDMI アダプターの不具合であろうか？もしそうであれば、HDMI コネクタを持つ MacBook を常に使うようにした方がいいのかもしれない。

このような「内蔵ディスプレイの画面更新が行われなくなる」という不具合は、NDI Screen Capture を使用したテレプロンプターシステムを運用した際にも生じていた。その時と共通しているのは、「主たるアプリをフルスクリーンで動かしている」という点と、「画面全体を何らかの手段でキャプチャーしている」という点であろうか。フルスクリーンモードに関する何らかの設定を見直す必要があるのかもしれない。
