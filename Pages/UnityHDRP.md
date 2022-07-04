# Unity HDRP

## HDR Output

- HDR 出力対応は HDRP 13 (Unity 2022.1) 以降
- Player Settings の "Use display in HDR mode" を有効にする
- Editor 上で有効化するには DX12 に切り替える必要がある（ビルドなら DX11 でも可）
- HDRP 側の tonemapper の設定を[適切に行う](https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@14.0/manual/HDR-Output.html)
- macOS では HDRP 側の対応が完了していない模様
