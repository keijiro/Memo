# AI Translation Services

機械翻訳および speech-to-speech モデルを使った吹き替え翻訳の試みについて記す。なお、現時点では日本語→英語の翻訳のみ模索している。

## Rask AI

https://www.rask.ai/

これまで試した中で最も有望なサービス。生成品質および UI/UX の面で優れている。もちろん完璧ではないが、かなり実用性が高いと感じられた。

### 作例

https://github.com/keijiro/Memo/assets/343936/c47b4171-c211-4f2f-b399-65d7e12bd81b

### できること

- 動画をソースとした吹き替え翻訳
- 音声をソースとした吹き替え翻訳
- ソースの声色の模倣 (voice cloning)
- 声色データのライブラリ化と再利用 (voice clone library)

### 長所

- UI がそれなりに洗練されていて直感的に使える。
- 使いたい機能が一通り実装されている。
- 生成品質はかなり高い。

### 短所

- レスポンスの不安定さは否めない（処理時間が長かったり短かったりまちまち）
- 特定の機能がに不具合がある（例：`.srt` 字幕ファイルを入力した吹き替え処理）
- 入力音声の文字起こしが完全ではなく手動での修正・調整が必須である。
- 価格設定がやや高めである。

### ワークフロー

今のところ安定して使用できているワークフローを記す。

#### 声色ライブラリの生成（省略可）

Rask AI の吹き替えは基本的にソース音声からの voice cloning が行われるが、声色を安定させたい場合は、使い回し可能な声色ライブラリを作っておいた方が良い。

- ノイズ除去、マスタリング済みの音声データを用意する。
- 音声データを最長10分、最大10MBに整える。これを複数用意しても良い。
- "Voice Clone Library" メニューから Voice Clone を生成する。

#### ソース映像の準備

Rask AI ではミックスダウン済みの完パケ映像をソースとすることも可能だが（謎の技術で背景の BGM 等は分離して処理される）、当然ながら高品質な吹き替えを望むならミックス前のデータを用いた方が良い。

リップシンクを行わないなら、映像無しの音声データ (`.wav`) をソースとしても良い。ただ、タイミング調整の都合から、低解像度でも映像は付けておいた方が良い。

#### 吹き替え処理（初回）

とりあえず最初は Rask AI に任せて全自動で翻訳処理を行わせる。本来はここで `.srt` 字幕ファイルを添付することで文字起こしのエラーを避けられるはずだが、なぜか現状では字幕ファイルを添付すると生成処理が無限ループに入ってしまう。とりあえずは字幕入力無しに完全自動で処理させるしかない。

#### 調整

以下の点を確認・調整していく。

- **文字起こしミスの修正** - 単純に文字起こしをミスっている場合があるので、確認し修正していく。
- **翻訳ミスの修正** - 英語の意味がズレてしまうケースも無論存在するので、注意深く確認し修正していく。
- **文の分割** - 文の区切りが正しく行われず繋がってしまっている部分を分割していく。分割しなくても吹き替えはできるが、タイミング調整をしにくいので、なるべく分割していった方が良い。
- **文の長さの調整** - 文の長さのミスマッチは Rask AI において最も違和感を生じやすいポイントとなる。文が長過ぎても短過ぎても、長さ合わせのために発音を詰めたり延ばしたりすることになり、それが「機械音声っぽさ」を生む結果となる。そのため、異なる英語の言い回しや、時には内容の追加・削除等を行なって、長さの調整を行なっていく必要がある。 Rask AI にはこれを AI (LLM) に解決させる機能が備わっているが、これは現状うまく機能していない（意味のないゴミで文字数を合わせようとしてしまう）。恐らくプロンプトエンジニアリングに失敗している。なお、Rask AI の UI には「この文は xx 文字に調整すべき」というようなガイドが表示されるが、これもまったく当てにならない。トライ＆エラーで調整していくしかない。

#### リマスタリング

調整が完了したら、吹き替え後の音声データ (`.wav`) をダウンロードし、動画編集ソフトに取り込んでリマスタリングを行う。オリジナル音声をダッキングしてミックスする等の処理も行えばより良い。

### 実用性

実用性はかなり高いと感じた。まだまだ手動での調整が必要とされる技術ではあるものの、これまで不可能だったこと（自分の声色での吹き替え翻訳）が可能になるという点には、他に代え難い価値がある。

なお、前述のような「ミスの修正」や「長さの調整」を行なっていく都合上、翻訳先の言語に詳しい必要がある。このワークフローで全く知らない言語へ翻訳するというのは、無理があるように思える。

あるいは、このようなサービスを使って翻訳吹き替え作業を行なってくれる業者が存在しても良いのではないかとも思った。文字起こしや翻訳にはどうしてもエラーが生じるものであり、ボタン一発で全部完璧に処理してくれるサービスというのは、当面の間登場するようには思えない。どうしてもここにはマンパワーが必要であり、それを供給するというのにも商売の余地はありそうだ。