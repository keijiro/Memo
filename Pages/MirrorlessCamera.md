# ミラーレスカメラ

主に動画用途での使い勝手を記す。

## Sigma fp

### 長所

- UX は全体的に良い。ボタンは少ないが UI の挙動は良好で、それほどストレスを感じない。
- 画質、カラーサイエンスも良好。
- フォームファクタ、重量も最高。

### 短所

- Log 撮影非対応。動画機として使うにはこれがキツい。
- Cinema DNG はストレージを食い過ぎる。気軽に使えるものではない。

### 動画機としての使い方

- 連続オートフォーカスの追従性は絶望的。手ぶれ補正も無いに等しい。その辺りの、一般的なミラーレス動画機に期待される機能はとことん不足している。
- ポストでグレーディングを行うには "Off" カラーモードを使うのが望ましいが、 Log ではないためダイナミックレンジは狭い。 Log のつもりで使うとハイライトが潰れがち。
- 動画機として実用性を持たせるには外部レコーダーが必須になると思われる。 Blackmagic Video Assist で BRAW 撮影を行う、など。
- Video Assist と外部 SDD を接続し BRAW ワークフローを構築することで化ける。廉価版シネマカメラとして実用的なものになる。「これはシネマカメラなんだ」と思い込めば、オートフォーカスや手ぶれ補正が使えないことは諦められる。コスパ的にも、本格的なシネマカメラを導入するのと比較すれば安く済む。

### Sigma fp L はどうか？

Sigma fp L は画素数を向上させた後継機だが、残念ながら動画では高画素を活かしにくい仕様になっているため、動画機として Sigma fp L を導入する利点はほとんど無いと言える。

具体的には、HDMI Raw 出力時の有効画素数低下が欠点として挙げられる。内蔵レコーダー（SD カードないしは外部 SSD への録画）では適切なフィルタリングが行われ高画素数を活かしたディテールが得られるが、HDMI 経由の外部レコーダーではラインスキップによるスケーリングが行われるため、ディテールが失われてしまう。このことは [Gerald Undone のレビュー](https://www.youtube.com/watch?v=Zsz3wto6lNk) で指摘されている。

前述の通り内蔵レコーダーを使用したワークフローには難があり、外部レコーダーの使用は必須となるが、その外部レコーダーで高画素数の利点が失われるとなれば、Sigma fp L は動画機として使えないという結論を下さざるを得ないだろう。

## Sony a6600

### 長所

- あらゆる面でソツなく無難な出来。
- USB 給電で動かせるのは何気に便利。
- S-Log ワークフローが使える。
- レンズの選択肢が広い。

### 短所

- 長時間撮影にはオーバーヒートの不安がつきまとう。一応、フリップスクリーンを開いた状態にしておけば大丈夫っぽいが……
- 画質は APS-C なりといった感じ。暗所で動画に使うのは難しい。特に S-Log を使うとどうしてもノイズが目立つ。
- ローパスフィルタの特性がいまいちな気がする（そもそもフィルタが無い？） S-Log 撮影した場合に偽色を生じやすい。

### 動画機としての使い方

- PP7 (S-Log2) を使う。 S-Gamut3 を使う必要は無いと言われており（そもそもセンサーが S-Gamut3 をカバーしていないとか）、 PP7 が最適と思われる。
- PP7 でディテールを少し上げておく。デフォルトは少しボヤけ過ぎのような気がする。

## その他の借りて使ったカメラ

### Sony a7 IV

言うまでもなく非常に使いやすい。 UI のレスポンスも良く、使用感はとても良い。

唯一の弱点はオーバーヒートを起こすこと。普通に 4K 撮影に使うと数十分で熱停止する。対策の方法は色々あるが、「いざという時に落ちる可能性がある」という事実は認識しておく必要がある。動画機としてこれを導入するのは不安感を伴う。

- 4K, 10-bit で Log 撮影し、Resolve 上では proxy を生成して編集する。
- さすがにフルフレームだけあって、Log 撮影してもノイズは滅多に出ない。a6600 のようなノイズに対する不安感は無い。
- オートフォーカスは完璧に近いので、フォーカス調整にかかる手間を無くすことができる。
- HDMI 出力がやや弱いか。グリッドやフォーカスピーキングなどのオーバーレイが乗せられないほか、ガンマ表示アシストを適用できないなど、肝心な要素が抜けている。

### Sony FX3

「熱対策の施されたα」といった感じのカメラ。動画機として導入するならこちらを選ぶべきだろう。
