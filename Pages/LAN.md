# LAN

## 簡易的なファイル転送

結局のところ `nc` (netcat) が一番使いやすい、という結論になった。

Windows 上で WSL にポートを開けるのは若干面倒なので、常に macOS 側で listen するのが良いと思われる。

### macOS から WSL へ転送

```
# on macOS
$ cat file.bin | nc -w 1 -l 9999
```

```
# on WSL
$ nc -q 1 192.168.0.x 9999 > file.bin
```

### WSL から macOS へ転送

```
# on macOS
$ nc -w 1 -l 9999 > file.bin
```

```
# on WSL
$ cat file.bin | nc -q 1 192.168.0.x 9999
```
