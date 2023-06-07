# WSL2

## Linux 側に立てたサーバーを他ホストからアクセス可能にする

PowerShell （管理者権限）で `netsh` を使って port proxy を作成する。

```
netsh interface portproxy add v4tov4 listenport=8999 connectport=8999 connectaddres=(wsl hostname -I).trimend()
```
