# WSL2

## Linux 側に立てたサーバーを他ホストからアクセス可能にする

1. Linux 側の eth0 （仮想イーサネット）のアドレスを調べる。

```
$ ip addr
```

2. Windows 側で port proxy を作成する。

```
netsh interface portproxy add v4tov4 listenport=8999 connectport=8999 \
  listenaddress=0.0.0.0 connectaddress=<eth0 addr>
```
