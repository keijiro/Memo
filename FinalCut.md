## プロジェクト管理方針

案件毎にライブラリを作る。つまり、実務上の「プロジェクト」の概念が、 FCP 上の「ライブラリ」に対応することになる。

案件毎にライブラリを分けるのは、その方がアーカイブ化しやすいため。案件が片付く度に、ライブラリを丸ごと圧縮してバックアップメディアにしまっていく。

## アーカイブ化手順

- Library Properties の Storage Locations 設定で Cache を外部化する。
  - 場所はどこでもよいが、今のところ `~/Movie` 下に「ライブラリ名＋ `Cache`」で作成するようにしている。
  - Delete Generated Library Files では Thumbnail Media が削除されないため、この手続きは必須になる。
- Delete Generated Library Files で中間生成ファイルを全削除する。
- `tar` で固める。
  - `tar --use-compress-program=pigz -c -v -f foo.fcpbundle.tar.gz foo.fcpbundle`
