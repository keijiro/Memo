# WSL2

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
