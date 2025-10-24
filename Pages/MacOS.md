# macOS Setup

[OSInstallation](OSInstallation.md)

# How to Share a Unity Build with others

Unity でビルドしたアプリを他人と共有する場合、セキュリティ (Gatekeeper) が問題になる。展示用のレンタルデバイスを使う場合などでは、セキュリティオプションを全て外してしまうという選択もできるが、他の人が日常的に使用している Mac の場合はそうも行かない。そういった場合はちゃんと署名の手続きを踏むことが適切な解決法となる。

署名の手続きは[このようなシェルスクリプト](https://github.com/keijiro/Fluo-Ring/tree/main/Misc)で半自動化できる。不明な部分や忘れた場合はチャット AI に聞けば良いだろう。

この手続きにおいて一点問題となるのは、公証処理において Apple のサーバーとの通信が必要になること。アプリの zip を丸ごと送信することになるので、それなりの帯域を確保しておく必要がある。 iPhone テザリングでは若干心許ないかもしれない。出張先や現場などで調整・ビルドを行い、その後署名を通すのは、かなり煩わしい作業になるだろう。そのような場合はさっぱりと諦めて、Gatekeeper を緩める方向で調整するべきかもしれない。

# macOS Oddities

macOS に関する奇妙なこと。

## Recording Indicator (Yellow Dot)

映像・音声入力等を用いると、画面の右上にオレンジ色のドットでインジケーターが表示される。これは外部出力にも適用されるため、映像送出用の装置として Mac を用いる際には大いに問題となる。

Sonoma より前のバージョンにおいてはこれを隠す方法が無かったため、[YellowDot](https://lowtechguys.com/yellowdot/) のような特殊なアプリを使ってこれを隠す必要があった。

Sonoma 以降では、少し特殊な設定になるが、公式にこれを隠す方法が提供されている。下記のページを参照のこと。

https://support.apple.com/en-us/118449
