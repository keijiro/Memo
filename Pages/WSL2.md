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

## ドライブ増設

２通りのアプローチがある

### 仮想ドライブ

VHDX 仮想ドライブイメージ内に ext4 パーティションを作成し WSL にマウントする、というアプローチ。

VHDX 仮想ドライブの作成までは Disk Management GUI 上で行うことができる。[こちらを参照](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/manage-virtual-hard-disks)。ただし、こうして作成された VHDX は管理者権限でアクセスが制限されている。以降の作業を簡便化するにはユーザーにフルアクセス権限を付与しておくのが良い。

こうして作成した VHDX は `wsl` コマンドでマウントできる。これ以降、WSL 側では物理ドライブとしてこれが見えるようになる。

```
wsl --mount --vhd D:\wsl2-ext.vhdx --bare
```

パーティションの構築には `gparted` 等を使用するのが無難。パーティションテーブルは msdos あるいは gpt を、ファイルシステムには ext4 を使用する。パーティション構築後は普通に `mount` コマンドでマウントできる。

```
sudo mount /dev/sdd1 mount-point
```

### 物理ドライブ

外部接続した物理ドライブに ext4 パーティションを作成し WSL にマウントする、というアプローチ。主に[このページ](https://learn.microsoft.com/en-us/windows/wsl/wsl2-mount-disk)の解説を参考にした。

まず、PowerShell 上でディスクパス（`\\.\PHYSICALDRIVE*` という形式で表されるもの）を特定する。

```powershell
GET-CimInstance -query "SELECT * from Win32_DiskDrive"
```

このディスクパスを使ってドライブを WSL へマウントする。

```powershell
wsl --mount \\.\PHYSICALDRIVE2 --bare
```

こうしてマウントしたドライブは WSL 上で `/dev/sd*` として見えるようになる。あとは `gparted` 等を使用してファイルシステムを構築すれば良い。パーティション構築後は普通に `mount` コマンドでマウントできる。

```
sudo mount /dev/sdd1 mount-point
```

### どちらのアプローチを採るべきか

速度的に大きな差は無いものと思われる。取り回しの容易さを考えると仮想ドライブの方が良さそうに思われるが、マウントの手順は案外に面倒で、当初考えていたほどに便利ではなかった。ドライブ全体を WSL に使用することが予め決まっているのならば、物理的に ext4 パーティションを作成する方が良いかもしれない。
