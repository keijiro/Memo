# macOS Oddities

macOS に関する奇妙なこと。

## Recording Indicator (Yellow Dot)

映像・音声入力等を用いると、画面の右上にオレンジ色のドットでインジケーターが表示される。これは外部出力にも適用されるため、映像送出用の装置として Mac を用いる際には大いに問題となる。

解決方法は２段階ある。

### YellowDot

https://lowtechguys.com/yellowdot/

オレンジ色のドットを白色あるいは黒色に変更することで目立たなくするツール。完全に隠すわけではないが、即席で目立たなくすることができる。

### Recording Indiacator Utility

https://github.com/cormiertyshawn895/RecordingIndicatorUtility

ドットを完全に隠すことができるが、本体のセキュリティ設定を変更する必要がある。僅かながらもセキュリティ上のリスクがあるうえ、借り物の Mac で適用するわけにはいかない、等の問題がある。

できれば YellowDot で乗り切り、どうしても品質上の要求により完全な除去が求められる場合のみ Recording Indicator Utility を使う、という選択になるだろう。
