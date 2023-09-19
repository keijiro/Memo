# Google Cloud Storage

Google Cloud Storage はデータのバックアップや巨大な納品データの受け渡しに使用している。

## 運用方針

[Autoclass 機能](https://cloud.google.com/storage/docs/autoclass)を使ってアクセス頻度の低いデータを低コストストレージへ自動的に移転していく。以前はこれを手動で行おうとしていたが、プロジェクトが完了した瞬間の見極めというのは難しく、どうしても古いデータが移転せずにそのままとなってしまっていた。 Autoclass を使うことで、そう言った煩わしさ無しにコスト削減を図ることができる。

Bucket, Object は基本的にすべて private 設定とする。共有・納品データについては、都度 signed URL を生成して対処する。

基本的にすべての操作は Storage Browser および Cloud Terminal から行うことができる。 Cloud Terminal のおかげで gsutil 等のコマンドをローカルにインストール必要が無くなった。

## Signed URL の生成

- Service account の JSON key が手元に無い場合は、まず[ここの手順](https://cloud.google.com/iam/docs/keys-create-delete)に従ってそれを生成する。
- Cloud Terminal を開き、準備を行う。

```console
keijiro@cloudshell:~$ pip install pyopenssl
keijiro@cloudshell:~$ export CLOUDSDK_PYTHON_SITEPACKAGES=1
```

Cloud Terminal から JSON key をアップロードする。

gsutil コマンドを使って signed URL を生成する

```console
keijiro@cloudshell:~$ gcloud storage sign-url gs://BUCKET_NAME/OBJECT_NAME --private-key-file=KEY_FILE --duration=1w
```

---

以下は古い情報。コストについては Autoclass の導入によって状況が大きく変化するものと思われる。

---

## コストについて

2022 年 8 月の時点で 1.6TB ほど使用していて、月に 5,000 円ほどのコストがかかっている。

Google One では月 1,300 円ほどのコストで 2TB 使用できる。それと比べると、だいぶコスパが悪いのは認めざるを得ない。

### 当初考えていたこと

Google Drive は 200GB プランの次は 2TB プランと大幅にジャンプしており、数百 GB しか使用していない場合はちょっと損しているように感じられる（気分の問題だが……）。また、2TB より上の追加コストは非線形に増加しており、増えれば増えるほどコスパが悪くなっていく。

Google Cloud Storage は、使用容量に応じて（GB/USD で）金を支払うという分かりやすさがある。また、古いデータから Coldline 等のアーカイブ用ストレージへ移していくことにより、段階的にコストダウンを図っていくことができる。

ゆえに、業務の中で大容量データの保管を行う場合は、Google Drive よりも Google Cloud Storage の方が適していると考えた。

### 実際には

- Google Drive の値下げ（特に 5TB, 10TB 帯）によりコスパが改善された。
- 意外と 2TB の壁は突破しなかった。
- Coldline の移行を怠ったため、そのコスパの良さを活かすことができていない。
- Google Cloud はドル立て請求のため、円安によるコスパの悪化があった。

今さら Google Drive に戻る気は起きないが、当初の目論見は外れつつあるというのが正直な感想。

せめて Coldline への移行を真面目に行うべきであろう。

## gsutil コマンドラインのメモ

### 特定フォルダ下のファイルをパブリックアクセス可能にする

`gsutil -m acl set -r -a public-read gs://bucket-name/folder-path`

### 特定フォルダ下のファイルを非公開に戻す

`gsutil -m acl set -r -a private gs://bucket-name/folder-path`
