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

## Linux 側に立てたサーバーを LAN 内でアクセス可能にする

Linux 側のポートを LAN へ露出するために `netsh` コマンドを使って port proxy を作成する必要がある。

```
netsh interface portproxy add v4tov4 listenport=8999 connectport=8999 connectaddres=(wsl hostname -I).trimend()
```

これを PowerShell スクリプト（例えば `wsl_portproxy.ps1` など）として保存する。管理者権限が必要となるので、実行にはショートカット等を使うのがよい。

なお、PowerShell スクリプトを実行するには、事前にセキュリティポリシーを変更しておく必要がある。

```
set-executionpolicy remotesigned
```
