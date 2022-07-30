# Google Cloud Storage

Google Cloud Storage はデータのバックアップや巨大な納品データの受け渡しに使用している。

## gsutil コマンドラインのメモ

### 特定フォルダ下のファイルをパブリックアクセス可能にする

`gsutil -m acl set -r -a public-read gs://bucket-name/folder-path`

### 特定フォルダ下のファイルを非公開に戻す

`gsutil -m acl set -r -a private gs://bucket-name/folder-path`
