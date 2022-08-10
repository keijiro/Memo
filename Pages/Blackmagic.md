# Blackmagic Design Products

Blackmagic Design のハードウェア製品について（Davinci Resolve については[こっち](Pages/DaVinciResolve.md)で）

## Video Assist

基本的にはカメラの外部レコーダーだが、小型モニター、 USB キャプチャーデバイス、 HDMI-SDI 相互変換器、等々のマルチな顔を持っている。

### Mac の HDMI 出力との相性

Mac からの HDMI 出力をキャプチャーすると、何故か少し緑っぽくなるという現象が発生する。以下の画像は、単純なグレイスケールのグラデーションを出力しキャプチャーしたもの。特に暗部が緑がかっている。

![grayscale](https://user-images.githubusercontent.com/343936/183838336-fd6c4eaf-91ff-4cc1-b12d-3feb36069fce.png)

原因はよく分かっていない。映像フォーマットやカラープロファイルを色々いじってみたけどダメ。

とりあえずこれはこういうものと諦めて、編集側でなんとかするしかないが、妙なカーブがかかっており、ホワイトバランス調整だけでは対処できない。グレースケール等のテストパターンを使って手動で補正するしかない。
