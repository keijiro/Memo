# Google Cloud Storage

Google Cloud Storage はデータのバックアップや巨大な納品データの受け渡しに使用している。

## 運用方針

[Autoclass 機能](https://cloud.google.com/storage/docs/autoclass)を使ってアクセス頻度の低いデータを低コストストレージへ自動的に移転していく。以前はこれを手動で行おうとしていたが、プロジェクトが完了した瞬間の見極めというのは難しく、また作業の煩わしさもあり、どうしても古いデータを移転せずにそのまま放置する状態となってしまっていた。 Autoclass を使うことで、そう言った煩わしさ無しにコスト削減を図ることができる。

Bucket, Object は基本的にすべて private 設定とする。納品データについては、都度 signed URL を生成して共有する。

基本的にすべての操作は Cloud Console および Cloud Terminal から行うことができる。 Cloud Terminal のおかげで `gsutil` 等のコマンドラインツールをローカルにインストールする必要は無くなった。

## Signed URL の生成

- Service account の JSON key が手元に無い場合は、まず[ここの手順](https://cloud.google.com/iam/docs/keys-create-delete)に従ってそれを生成する。
- Cloud Terminal を開き、基本セットアップを行う。

```console
keijiro@cloudshell:~$ pip install pyopenssl
keijiro@cloudshell:~$ export CLOUDSDK_PYTHON_SITEPACKAGES=1
```

- Cloud Terminal から JSON key をアップロードする。
- `gsutil` を使って signed URL を生成する

```console
keijiro@cloudshell:~$ gcloud storage sign-url gs://BUCKET_NAME/OBJECT_NAME --private-key-file=KEY_FILE --duration=1w
```
