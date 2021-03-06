各種データのバックアップ方法について。

# 基本戦略

プロジェクト開発中は、それぞれのプロジェクトの都合に合わせて最も簡便な手段を使ってバックアップおよびバージョニングを行なっていく。

Unity プロジェクトであれば GitHub か Plastic SCM を使う。アセット量の多さに合わせてどちらか選択するという感じ（多ければ Plastic を使う）。

動画編集であれば、ローカルの HDD/SDD への単純コピーというアドホックな方法を使うことが多い。

そしてプロジェクト完成後に、アーカイブ用の安価なストレージへ移動を行う。今の所 Google Cloud Storage を主に使用している。

Google Cloud Storage は Nearline や Coldline 等の、アーカイブ専用の安価なストレージクラスが用意されている。プロジェクト完成から長期間経過したプロジェクトはこれらのアーカイブ用クラスへ移動することにより料金を抑えることができる。

……という理屈だが、今の所まだ Nearline/Coldline を使用したことはない。通常のクラスでもさほど高額な負担にはなっていないため。いずれ検討する時期が来るかもしれない。

## Unity の場合

巨大アセットは専用のパッケージに格納して、そこだけはバージョン管理システムではなく手動バックアップで管理する。それ以外の基本的なアセットは、通常通り git で管理。

……というアプローチがいいかもしれない（まだ実際に運用したことは無い）

# Borg

https://borgbackup.readthedocs.io/

Borg はコマンドラインベースのバックアップ管理ソフトウェア。バージョン管理システムほどの柔軟性は無いが、巨大なプロジェクトのスナップショットを高速に作成する事ができる。

一時期、この Borg を巨大プロジェクトのバックアップに使用していた。Borg で適宜スナップショットを作成し、Google Cloud Storage へ同期保存するというアプローチ。

使い勝手は決して悪くないし、巨大プロジェクトでは理想的なソリューションであるとも思うが、最近はあまり使っていない。一番の理由は、しばらく使っていないと使い方を忘れるから、というもの。やはりこの手のツールは日常的に使っていないと、再び使い出すのが億劫になってしまう。

以下は過去に使用していた際に作ったメモ。また使う機会があるかもしれない。

#### Why do you prefer [Borg] over [git-lfs]/[git-annex]?

- GitHub LFS has a 2GB limit on file size. This never works with large projects
  like video productions.
- git-annex uses symlinks to manage annexed files. This doesn't work well on
  Windows.

So my conclusion at the moment is that Borg is the best backup software for
mid/large-sized personal projects.

[Borg]: https://borgbackup.readthedocs.io
[git-lfs]: https://git-lfs.github.com/
[git-annex]: https://git-annex.branchable.com/

#### Weren't you using [rsync.net]? Why did you switch to Google Cloud?

rsync.net is quite handy for using with Borg, but it's slow from Japan. The
server is located in Hong Kong, and the connection speed varies through the day.
It might be a good solution for people in North America, Western Europe and
China, but unfortunately it doesn't work well from my workplaces.

I should admit that Google Cloud Storage is super fast and stable. It also
provides long-term/low-cost storage classes (nearline/coldline/archive) that are
cost beneficial for backup use.

[rsync.net]: https://www.rsync.net/

#### Is Borg flexible enough for version control?

No. Although Borg is useful for taking project snapshots, it doesn't provide
any version control features like git or hg. When I need to track file versions,
I create a git repository in a subdirectory and make it contained in the Borg
repository.

## Installing gsutil

- Linux and Windows (WSL): Install pip `sudo apt install python3-pip`,
  then install gsutil `sudo pip3 install gsutil`.
- MacOS: Install pip `sudo easy_install -U pip`, then install gsutil
  `pip install gsutil --user`. The `user` flag is needed to avoid a package
  conflict problem with `six`.

After installing `gsutil`, authenticate it with `gsutil config`.

## Creating a Google Cloud Storage bucket

Go to [Google Cloud Console](https://console.cloud.google.com) and create a
bucket.

Create `Borg` folder in the bucket. The Borg repository files will be copied
into this folder.

## Installing Borg

- Linux: `sudo apt install borgbackup`
- Windows (WSL): Install the `borgbackup` apt package from
  [PPA](https://launchpad.net/~costamagnagianfranco/+archive/ubuntu/borgbackup).
  Don't use the Ubuntu package. Some issues on WSL are only fixed in the very
  recent versions.
- MacOS: `brew cask install borgbackup`.

Then create a borg repository:

`borg init --encryption=none Borg`

## Borg related files

Download `borg.sh` from this gist or run the following command line.

`curl -L https://git.io/Jv5k0 > borg.sh`

Open `borg.sh` with a text editor and modify `BUCKET_NAME` to the Google Storage
bucket name. Then put it in the project root directory, and make it executable
using `chmod`.

Run the following command line to initialize the Borg repository.

`./borg.sh init`

It's recommended to upload `borg.sh` to the Google Storage bucket because it'll
be needed to restore the repository in another environment.

## Taking a project snapshot

`./borg.sh push`

# Plastic SCM

### Taking a snapshot of a Plastic SCM workspace with tar

On WSL:

```
cat ignore.conf | sed -e s/^/./ | tar -I pigz -c -v --exclude=.plastic -X - -f ../snapshot.tar.gz .
```

### Making a backup of a Plastic SCM repository using the fast-export format

On PowerShell:

```
cm fast-export RepositoryName backup.fast-export
```
