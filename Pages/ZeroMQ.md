https://zeromq.org

近代的な設計の socket という感じ。プロセス間通信に超便利。

## UDP

UDP 対応は experimental であるとのこと。実際、 Rust の ZeroMQ 実装は UDP 非対応だった。

## ライセンス

現状は LGPL なので若干注意が必要。将来的に Mozilla Public License に移行予定らしい。

## 使ってみた

https://github.com/keijiro/vzo

DAW から OSC を送信するための VST プラグイン。 複数のプラグインインスタンス間で UDP ポートを共有するため、UDP ポートを管理するブリッジプロセスを独立して立てる設計にした。そのブリッジプロセスとプラグイン間の通信に ZeroMQ を利用した。

言語は Rust を使用。むちゃくちゃ簡単に使えてびっくりした。唯一の弱点は `libzmq` に依存を持ってしまうこと。アプリとして配布するにあたってどうパッケージングするべきか、未だよく分かっていない。

## Rust で GUI アプリは作れるのか？

https://www.areweguiyet.com

2022 年現在、 Rust で GUI アプリを作るための定番ソリューションは未だ存在していない、というのが結論になる。

個人的には [egui](https://github.com/emilk/egui) に親近感を覚えるが、いかにも GL 系の GUI ライブラリという色合いがあって、一般的なアプリを開発するのに適しているかどうか分からない。
