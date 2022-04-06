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
