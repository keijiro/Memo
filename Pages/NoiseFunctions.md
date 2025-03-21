# ノイズ関数

様々な使い道のあるノイズ関数。実装のバリエーションも幅広く、色々混乱してきたので、まとめてみる。

## Value Noise vs Gradient Noise

Value noise の見た目の悪さは視覚化した際に明らかだが、そもそも何が問題なのか。

- 周波数成分を絞り込むことができないので、低周波成分だけの「滑らかなノイズ」を作ることができない。また、フラクタルノイズにも使えない。
- 画像や動きにノイズを用いる場合、Gradient Noise の持つ周期性が好ましい結果を導く傾向にある。Value noise には周期性が無い。

これらが問題にならない場面では value noise も使えると考えられる。実際に使ったことはあまり無いが……

## Classic Perlin Noise

Ken Perlin の実装には、流石に古すぎて現代では通じない部分がある。

- Perm テーブルの長さを 257 にして `& 0xff` を省略するとか、もうしなくていいんじゃないかな。
- 勾配の方向を対角線上に絞っているが（内積計算を簡単にするため）、こういう縛りも必要無いかも。

しかしそれでも、油断するとオリジナルの実装に勝てなかったりする。なんだかんだで CPU 上ではいまだにインストラクション数の絞り込みが重要であるため、ギリギリまで削ぎ落とされた Ken Perlin の実装は、今でもある程度の優位性を持っている。

（もちろん、これらは GPU では通用せず、シェーダー実装するならば、あらゆる箇所を直した方が良い）

### Perlin Noise の値域問題

https://digitalfreepen.com/2017/06/20/range-perlin-noise.html

２次元の場合は理論的な最大値に達する可能性がある。この解析解を参考にすれば良い。

問題となるのは３次元の場合。理論的な最大値に達するには、近接する８つの勾配ベクトルが全て中央を向く必要がある。これは確率的に滅多に起こらない。そのため、理論的な最大値で正規化すると、だいぶヘッドルームが生じることになる。

## 乱数生成の手法

Gradient Noise では勾配ベクトルの方向を決めるために乱数生成器を使用する。この乱数アルゴリズムは、CPU/GPU のいずれにおいてもパフォーマンスに影響を与えうる要素であり、また見た目にも影響を与えうるため、選択が重要となる。

オリジナルの Perlin Noise は Perm テーブルを使用した古典的な方法で、これは CPU 上では未だに妥当な選択となりうる。ハッシュアルゴリズムでこれを超えるのはなかなか難しい。ただし、GPU 上で実装するにはあまり魅力を感じない。メモリアクセス／フットプリントの両面から欠点が目立つ。

webgl-noise の実装は非常に巧妙でスマートである。選定の背景について[ちゃんと解説が用意されている](https://github.com/stegu/psrdnoise/tree/main/article)のも有り難い。興味深い事実として、この実装は[定数の更新が何度かされている](https://github.com/stegu/webgl-noise/commit/c008e21d3df2ab0b45aaa0c86df4a817ad6b95d4)ことが挙げられる。初期の段階で webgl-noise から派生したコードは、この変更が反映されていなかったりする。

ハッシュ関数を乱数生成に用いる方法も考えられるが、上記の２方式に速度で勝つことは非常に難しい。多くの場合は webgl-noise のアプローチを引用するのが望ましいのではないか。

## Unity の実装

まず、標準的な関数として、 `Mathf` に `PerlinNoise` と `PerlinNoise1D` が用意されている。非常に基本的な実装であり、これらを積極的に使用する理由はあまり無い。

`Unity.Mathematics` には `noise` クラスが実装されている。この中には webgl-noise から移植されたコードが一通り揃っている。シェーダーからの移植となるが、オリジナルの実装は SIMD との親和性を意識されたものであるため、Burst に最適化するという文脈では意味のあるものになっている。もちろん、Burst を使用しないとかなり重い。ちなみに、最近の webgl-noise における更新はあまり反映されていない。

VFX Graph 内のノイズ関数の実装は基本的に独自実装だが、コードを覗くと webgl-noise の面影が感じられるものとなっている。勾配のバリエーションを増やした Perlin Noise の派生形であり（なので "Perlin Noise" と銘打っているのは、厳密には正しくないと思う）、ノイズ値と同時に微分値も算出できる工夫がなされている。これは素直に偉い。ただしハッシュ関数があまりにも簡素で少し心配になる。もしかしたらかなり癖（パターン）が現れているのではないだろうか。

## webgl-noise

https://github.com/ashima/webgl-noise

Classic Noise （Perlin Noise の派生形で、勾配ベクトルのバリエーションを増やしたもの）と Simplex Noise の非常に効率の良い実装。元の意図は GPU に向けたノイズ関数の最適化だが、単純にノイズ関数の実装として非常に効率が良いことと、わかりやすい Simplex Noise の実装が希少であったことから、CPU/GPU を問わず多種多様な場面で流用されている。

ただし、このオリジナル実装のリポジトリの管理者である Ashima Arts 社は既に倒産しており、引用元として使用すべきかどうかは怪しい。作者によって更新されているものの、いつまで管理されるかは分からない。

webgl-noise の実質的な作者である stegu 氏が現在管理しているリポジトリが下記にある。

https://github.com/stegu/webgl-noise

同氏はのちに psrdnoise を開発している。Simplex Noise に勾配ベクトルの回転機能を加えたものになる。

https://github.com/stegu/psrdnoise

この psrdnoise のコードは一部 webgl-noise にも取り込まれているが、psrdnoise の方は論文やデモ、チュートリアルが充実しており、実装の背景を知るのに役立つ。

### webgl-noise の欠点

webgl-noise は元々 SIMD を意識した実装内容になっているが、現代の GPU は SIMD を指向していないため、あまり意味の無い部分もある。

`step` を使用している箇所があるが、これは GPU によってペナルティになる場合がある。

`normalize` や `sin`/`cos` を避けるためにテイラー展開された `rsqrt` を使用しているが、これは不要な最適化かもしれない。

Unity に移植する際にはこの辺りを単純化した。

## 自作ライブラリの現状

まばらに実装したため、パッケージによって使用しているものが異なり一貫性が無い。

### [NoiseShader](https://github.com/keijiro/NoiseShader)

`webgl-noise` の比較的シンプルな移植になる。ただし sin/cos や normalize 等、使用に問題がないと思われる関数を積極的に使用する形に直しつつ、HLSL 的に平易な書き方になるよう調整を行なった。

できればサンプルを URP で書き直したい。また、1D noise を追加したい。

### [KlakMath](https://github.com/keijiro/KlakMath)

自作のシンプルな 1D Gradient Noise 実装を持っている。

### [ProceduralMotion](https://github.com/keijiro/ProceduralMotion)

Brownian Motion コンポーネントで `Unity.Mathematics` のノイズ関数を使用している。

これは 1D Gradient Noise に変更すべきかもしれない。ノイズフィールドのスライスを使用する欠点が現れてしまっている気がする。

### [ProceduralMotionTrack](https://github.com/keijiro/ProceduralMotionTrack)

ProceduralMotion と状況はほぼ同じ。

### [PerlinNoise](https://github.com/keijiro/PerlinNoise)

大昔に組んだもので今は利用価値が無い。
