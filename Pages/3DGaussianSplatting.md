# 3D Gaussian Splatting

## Unity で使う

Unity で 3D Gaussian Splatting を扱う方法には大きく分けて２種類ある。

- [UnityGaussianSplatting](https://github.com/aras-p/UnityGaussianSplatting/)
- [SplatVFX](https://github.com/keijiro/SplatVFX)

パフォーマンスでは UnityGaussianSplatting の方が高く、場合によっては倍以上の速度差がある。主な違いはソートアルゴリズムにあり、SplatVFX では最適化されていない汎用のソーティング機構（3DGS 向けに最適化されていない、の意。汎用的なパーティクルシステム向けに実装されている）を用いるため、どうしても差が出てしまう。ただし、UnityGaussianSplatting は対応プラットフォームに若干の狭さがあり、BiRP on DX12 であればフルスペックで動くものの、その他の組み合わせでは何らかの制約が生じがちである。
