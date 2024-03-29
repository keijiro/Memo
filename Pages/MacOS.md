# macOS Setup

初期セットアップとして行なっていること

- ホスト名設定
- [dotfiles](https://github.com/keijiro/dotfiles)
- [brew](https://brew.sh)
- [Lilex Nerd Font](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Lilex)
- [kitty](https://sw.kovidgoyal.net/kitty/) (via brew)
- [kitty.conf](https://gist.github.com/keijiro/a5d061397041c7bc754bae1e55bf2098)
- [neovim](https://neovim.io) (via brew)
- [nvim-config](https://github.com/keijiro/nvim-config)
- [ripgrep](https://github.com/BurntSushi/ripgrep) (via brew)
- [git-lfs](https://git-lfs.com) (via brew)

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

ドットを完全に消すことができる。

ただし、本体のセキュリティ設定を変更する必要がある。僅かながらもセキュリティ上のリスクがあるうえ、借り物の Mac で適用するわけにはいかない、等の問題がある。できるだけ YellowDot で乗り切り、どうしても品質上の要求により完全な消去が求められる場合のみこちらを使う、というような選択になるだろう。
