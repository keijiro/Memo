# OAK-D-Lite (OpenCV AI Kit - Lite)

https://docs.luxonis.com/projects/hardware/en/latest/pages/DM9095.html

[Kickstarter](https://www.kickstarter.com/projects/opencv/opencv-ai-kit-oak-depth-camera-4k-cv-edge-object-detection
) で購入。割引で $89 也。

ステレオカメラ＋機械学習推論プロセッサ。 Myriad X をベースにしており OpenVINO モデルを使用できる。分かりやすく言えば「AI 内蔵 USB カメラ」。

# 深度カメラとして

深度も取得できるが IR 無しのステレオカメラなので先行製品と比較して精度は低い。カラーと深度の同期も結構面倒臭い。深度カメラとしてはあまり過度に期待してはいけない。

# テストプロジェクト

https://github.com/keijiro/DepthAITestbed

プラグインのビルドには `brew install libusb` が必要。

## ビルド環境

- macOS Apple silicon: 成功
- macOS universal: 失敗。 depthai-core が universal に未対応。 example のビルドも失敗する。
- Windows (mingw32 on WSL2): 失敗。 [thread 問題](https://stackoverflow.com/questions/14191566/c-mutex-in-namespace-std-does-not-name-a-type) のため。
- Linux: 失敗？ `depthai-core` をビルドする際に `cmake` に `"-DBUILD_SHARED_LIBS=ON"` を渡せば、プラグインのビルドまでは進める。 Editor 上でも１度は動かすことができるが、 play mode から edit mode への復帰時にクラッシュする。ただしログへの出力は何も無し。 Standalone Player は起動時に SIGXCPU で止まる。 Editor がクラッシュする現象も SIGXCPU かも？

今の所、 macOS 上での non-universal か、 Windows 上での VS with cmake でしかビルドできない。

# 開発

普通は Python, OpenCV などを組み合わせて使う。それらを普段から使っている人であれば容易に使いこなせるはず。

そうでない場合はセットアップが面倒かもしれない。 M1 Mac では Intel ベースで brew を入れなければいけないなど、複雑な依存関係に起因する面倒さが散見される。

OpenCV は主にサンプルプログラムが依存している状態で、素の状態から直接使うなら排除できる。 Unity プラグインは OpenCV 無しで作れることを確認した。ただし、現状では libusb に依存してしまっている（恐らく使用時に brew のインストールが必要になる）。

依存が複雑なのでビルドプロセスも複雑なことになっているが、 cmake でのビルドが整備されているので、 cmake を使う限りにおいては比較的簡単に使用できる。

# 強み

使ってみた感想として、オープンソースであることが最大の特徴であると感じた。機能に制限が多くとも、細かい点まで自分で調整して納得いくまで妥協点を探していくことができる。ハックするのが楽しい自由なハードウェア。

# 何に使うか

深度カメラとしてはあまり魅力は感じないが、 RealSense は将来的に入手不可能になるし Azure Kinect も品薄状態に陥っているため、消去法として OAK-D を使用する状況になるかもしれない。

ポーズ推定専用カメラとして割り切って使うのも面白そう。 VTuber 用カメラとして。