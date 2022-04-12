# DaVinci Resolve

## YouTube

[YouTube は BT.709 1-1-1 を前提としている](https://support.google.com/youtube/answer/1722171?hl=en#zippy=%2Ccolor-space)（これは Resolve 上の Rec.709-A に相当する）ため、普通の設定（結果的に BT.709 1-2-1 になる）で出力すると、エンコード時に色が薄くなるという現象が発生する。

これを避けるには、 Resolve 上でも Rec.709-A で編集を行う必要がある。

最も簡単なのは、[この動画](https://www.youtube.com/watch?v=8tiF-EnTlto)の設定をそのまま使うこと。

ただし、これは既に Rec.709 で調整されてしまったプロジェクトには適用できない。

既に Rec.709 で調整済みのプロジェクトを Rec.709-A に補正する正式な方法は存在しない（と思われる）。 Timeline の出力を Rec.709-A に変更したうえで、 Adjustment Clip 等を使って手動で再調整を行うしかない。

## 定型ワークフロー

### General Preferences

- General Preferences で "Use Mac display color profiles for viewers" を入れる。

### PProject Settings

- Color Management Color science を "DaVinci YRGB Color Managed" に変更。
- "Automatic color management" を外す。
- Color processing mode を "HDR DaVinci Wide Gamut Intermediate" に変更。
- Output color space を "Rec.709-A" に変更。
- Master Settings を納品フォーマットに変更。
- Fairlight で Target loudness level を -13 LUFS に変更。

## うまくいかなかったこと

- Sync Bin を使ったマルチカム編集は自分には合わなかった。 Sync Bin では入れ子のタイムラインや Compound Clip を使うことができないため、コンポジット済みの素材を編集するには向いていない。チュートリアル動画などでは、クロマキー合成を行った後の素材を細かくカット編集していくことになるため、この「向いていない」ケースになる。
- ならば、コンポジット済みの素材を中間ファイルに一旦エクスポートしてから再取り込みすればいいのではないか、となるが、これはエクスポートに時間がかかるためあまり現実的ではない。１〜２時間の撮りっぱなし素材を扱うことが多いため、非常に無駄が多いワークフローとなってしまう。また、再調整のしずらさも大きな欠点。
- このように、「中間ファイルに一旦吐き出せば綺麗に解決できる」というのは、あまり良いアイデアではない。特に長弱編集には適していない。もし、どうしても中間ファイルを使いたいならば、カット編集を済ませて尺が短くなった後に行うよう、ワークフローを工夫するべき。
- 入れ子のタイムラインは非常に便利だが、あまり入れ子の中身で複雑なことをやり過ぎてしまうと、編集時のソフトウェアの動作が重くなってしまう。複雑な処理はなるべく入れ子の上段に逃がすよう工夫する。
- ループ背景素材のような単純なものに入れ子のタイムラインを使うと余計な負荷になってしまう。外部で編集を済ませておくこと。

## アーカイブ化手順

- Project Manager から "Export Project Archive..." を実行。このとき Render Cache は除外しておく。
- `tar` で固める。
  - `tar --use-compress-program=pigz -c -v -f Foo.dra.tar.gz Foo.dra`

## Tips

- Fairlight の Noise Reduction エフェクトは逆再生のパフォーマンスを著しく低下させる。カット編集を細かく行う（いわゆる「ジェットカット」などのような）場合は、最終調整までこれを入れないようにすべき。カット編集用の入れ子のタイムラインを使った方がいいかもしれない（子のタイムラインでカット編集を行い、親のタイムラインで音の調整を行う）。
- Optimized Media を削除するには `CacheClip/Optimized Media` フォルダを[手動で削除しなければならないらしい。](https://forum.blackmagicdesign.com/viewtopic.php?f=21&t=136275#p734817)
