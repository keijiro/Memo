# Unity Multi Display

Unity でのマルチディスプレイの使用について

## 基本

[Display](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/Display.html) クラスを通して制御する。デスクトップにおける使用方法は１０年近く変わらない。その後 iOS や Android での対応が付け足されたが、個人的にこの辺りは全く未検証なのでよく分からない。

アプリ起動後、`Display.displays` を調べて、表示したいディスプレイについて `Activate` を実行していく。あとは `Camera` の `targetDisplay` を設定するだけでレンダリングを始められる。

基本的な使い方は簡単だが、使いこなそうとすると様々な問題が発生する。また、それに対する解法が必ずしも用意されているとは限らないのが、何とも悩ましい。
