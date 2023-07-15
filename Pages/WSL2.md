# WSL2

## ファイルシステム問題

Windows-Linux 間では相互にファイルシステムの参照が可能だが、アクセス速度はかなり遅い。そのため、ファイル／ディレクトリを主にどちらのシステムにおいて使用するかを踏まえて置き場所を決める必要がある。

普段の Unity プロジェクトの管理においては次のような葛藤がある。

- `git` の速度を最適化するには Linux 側に置きたい。
- しかし Unity Editor はプロジェクトを Linux ファイルシステム上に置くことを許可していない（クラッシュする）

ひとまず次のような折衷案を導き出した。

- `git` リポジトリのローカルコピーは Linux 側に置く。
- Windows 側に空のプロジェクトディレクトリを作成し、`Assets`, `Packages`, `ProjectSettings` へのシンボリックリンクを作成する。

これにより、Unity Editor は主に Windows 側で動作させつつ、アセットの管理・編集は Linux 側で高速に行うことが可能になった。

この作業用 Windows プロジェクトディレクトリ構築を行うためのシェルスクリプトを作成した [make-unity-symlink](https://github.com/keijiro/dotfiles/blob/master/bin/make-unity-symlink)

### 欠点

Unity Editor から各アセットファイルを読み出す際の速度は低下するので、巨大なプロジェクトをこの手法で管理するのは望ましくないだろう。結局のところ、巨大プロジェクトについては Git for Windows をインストールして Windows 側で管理を行うようにすべき。

## Linux 側に立てたサーバーを LAN 内でアクセス可能にする

Linux 側のポートを LAN へ露出するために `netsh` コマンドを使って port proxy を作成する必要がある。これを行うためのシェルスクリプトを作成した [make-portproxy](https://github.com/keijiro/dotfiles/blob/master/bin/make-portproxy)
