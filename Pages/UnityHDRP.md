# Unity HDRP

## HDR Output

- HDR 出力対応は HDRP 13 (Unity 2022.1) 以降
- Player Settings の "Use display in HDR mode" を有効にする
- Editor 上で有効化するには DX12 に切り替える必要がある（ビルドなら DX11 でも可）
- HDRP 側の tonemapper の設定を[適切に行う](https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@14.0/manual/HDR-Output.html)
- macOS では HDRP 側の対応が完了していない模様

## Water System

- Changelog 上では HDRP 13 (2022.1) に記載されているが、今のところは HDRP 14 (2022.2) でないと有効化できない模様。
- デフォルトでは機能がオフになっているので、色々いじくってオンにしていく必要がある（詳しい手順は忘れた……）。
- 情報は [Forum](https://forum.unity.com/threads/water-system-for-the-high-definition-render-pipeline.1203751) で提供されている。

## Platform API

- DX12 は 2022.2 以降 experimental タグが外れて正式機能になる。

## DXR

- Visual C++ Runtime がインストールされていないと raytracing shader のコンパイルに失敗するという不具合が発生している（2022.1 以降）。
- 2022.2 から pathtracing において OptiX, Open Image 等の denoiser が使えるようになった。これにより pathtracing は一気に実用レベルになった。
