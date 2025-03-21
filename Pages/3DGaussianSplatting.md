# 3D Gaussian Splatting

## Unity で使う

Unity で 3D Gaussian Splatting を扱う方法には大きく分けて２種類ある。

- [UnityGaussianSplatting](https://github.com/aras-p/UnityGaussianSplatting/)
- [SplatVFX](https://github.com/keijiro/SplatVFX)

パフォーマンスでは UnityGaussianSplatting の方が高く、場合によっては倍以上の速度差がある。主な違いはソートアルゴリズムにあり、SplatVFX では最適化されていない汎用のソーティング機構（3DGS 向けに最適化されていない、の意。汎用的なパーティクルシステム向けに実装されている）を用いるため、どうしても差が出てしまう。ただし、UnityGaussianSplatting は対応プラットフォームに若干の狭さがあり、BiRP on DX12 であればフルスペックで動くものの、その他の組み合わせでは何らかの制約が生じがちである。

SplatVFX は VFX 的な応用がしやすいというメリットもある。しかし、3DGS は純粋な点群ではないため、これを無理やり VFX に使うのは筋が悪いという見方もある。

### 今後の展望

SplatVFX を UnityGaussianSplatting のアセット形式に対応させるべきだろう。クラスタリングによりパフォーマンスが向上するかどうかも検証したいところ。

UnityGaussianSplatting はソート処理が完全に分離された２パス形式であるため、ソート部分だけを借用して、レンダリングは VFX Graph で行う、という手もある。ただしこの場合、ソート処理の互換性の低さをデメリットとして受けることになる。例えば WebGPU では動かなくなってしまう。

SplatVFX は Custom　HLSL の機能が無い頃に実装したものであり、Custom HLSL の導入によってどの程度改善が可能であるかも検証したい。

## ソート問題

3DGS のレンダリングにあたって最も問題になりやすいのは、半透明描画のソートをいかに高速に行うか、という問題。

[splat](https://github.com/antimatter15/splat) では、ソートを非同期に CPU 実行するというアプローチをとっている。これは、視点が急激に移動してソート順序が変化するような状況は稀である、という前提で実装されている。実際のところ、インタラクティブなアプリにおいてはほぼこれで問題無いように思われる。

## ファイル形式

[SPZ](https://scaniverse.com/news/spz-gaussian-splat-open-source-file-format) という圧縮形式が提唱されているが、現状それほどアドバンテージは高くないように思われる。

なにしろ対応ソフトが少ないので、特定のファイル形式が大きくアドバンテージを得るという状況にはないように思われる。何かしら非常に使いやすいアプリケーションが現れた場合には、そのアプリの専用形式が一気にメジャーになる、というような展開にもなるかもしれない（現状ではそうったことはない）
