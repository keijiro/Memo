# Unity ECS

## Time 制御まとめ

### 最大 delta time はどうやって決まるのか？

- 基本的には [World] の `MaximumDeltaTime` プロパティによって決まる。
- ただし Default World の `deltaTime` は [UpdateWorldTimeSystem] によって外部制御されている。
- そのためプロジェクト設定の Maximum Allowed Timestep が間接的に反映されることになる。
- 結論として `World.MaximumDeltaTime` か Maximum Allowed Timestep のいずれかの小さい方の値が反映されることになる。

[World]:
  https://docs.unity3d.com/Packages/com.unity.entities@1.0/api/Unity.Entities.World.html
[UpdateWorldTimeSystem]:
  https://docs.unity3d.com/Packages/com.unity.entities@1.0/api/Unity.Entities.UpdateWorldTimeSystem.html

### Physics の time step はどうやって決まるのか？

- Unity Physics は [FixedStepSimulationSystemGroup] で駆動する。
- [FixedStepSimulationSystemGroup] の Timestep のデフォルト値は 1/60 であり、多くの場合これがそのまま用いられる。

[FixedStepSimulationSystemGroup]:
  https://docs.unity3d.com/Packages/com.unity.entities@1.0/api/Unity.Entities.FixedStepSimulationSystemGroup.html

## Unity Physics

- カスタム処理を挟み込むためのフックや、内部情報にアクセスするための API が充実しているため、カスタマイズの幅が広いのは確か。ただ、API の使用方法が詳しく解説されているわけではなく、サンプルもそれほど充実しているわけでは無いため、実際にカスタマイズしようと思ったら、かなり内部構造に詳しくなる必要がある。そのため「カスタマイズしやすい」と言うのとは少し違うような気がする。「カスタマイズを意識した設計になっている」と言うのが現実的な表現になるか。
- ステートレスであるのは、ゲーム用途では確実に便利なポイント。位置と速度さえ保存すればロールバックが簡単にできる。
- コリジョン／トリガーのイベント処理の書き難さが欠点の一つであると感じた。イベントとその対象となるエンティティ／コンポーネントを結び付けるまでの行程が長い。 OOP ならば、コールバック関数にイベント対象が引数として渡されて、その応答を関数内に実装し……と、非常に直接的な記述ができた。ECS ではそうは行かない。個々のイベントに注目するのではなく、イベント全体を群として捉える訓練が必要（ECS では何でもそうなるんだが……）
- 小・中規模なシーンであれば PhysX と同程度のパフォーマンスで動作する。数が多くなってくると流石に不利になってくる。
- Havok Physics は確実に速い。 PhysX や Unity Physics の４〜５倍は速い印象。

## Source generators

Entities パッケージにおいて source generator は以下の機能を提供するために用いられる。

- Entities.ForEach
- Job.WithCode
- Aspects
- Idiomatic C# `foreach`
- SystemAPI
- IJobEntity

これらの generator は新規の型や partial 型、メソッドの追加を行うことでこれを実現する。特定の条件下においては、コードの追加だけでなく既存コードに対するパッチングも行われる（ただしこれは別途 Cloner IL posotprocessor によって行われる）

Generator のソースコードは `Unity.Entities/SourceGenerators/Source~/SystemGenerator` 下に格納されている。

## 疑問点の確認

### ECS のオーサリングは必ず subscene 内で行わないといけないのか？

Subscene を使わなくても ECS の API を叩いて entity/components を生成する方法はあるが、通常の baking を使ったワークフローではなくなる。通常の Baker を使ったワークフローを「オーサリング」と定義するならば、オーサリングに subscene は必須であると言えよう。

### Subscene 内でオーサリングしたものが ECS に変換される？

厳密な表現を行うならば「Authoring subscene 内に組み立てた Game Object ベースのヒエラルキーが baking によって、entity subscene へと変換される」となるか。表層だけを見て言い表すなら「Subscene にオーサリングしたものが ECS に変換されて動く」と言えるが。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/conversion-scene-overview.html

### Subscene のチェックボックスを入れる操作は「ロード」？

正しくは "Open" と言うべきか。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/conversion-subscenes.html

### Entity は本当に単なる ID?

実際には Version 値も持っているが、実質的には ID と考えてよいだろう。ドキュメント上でもそう説明されている。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/concepts-entities.html

### Serialization/Deserialization は本当に速いのか？

ECS における serialization は以下の２レイヤーに分けられる。

`Unity.Entities/Serialization/DotsSerialization.cs` - RIFF 的なファイルフォーマットを定義したもの。コンテナ。

`Unity.Entities/Serialization/SerializeUtility.cs` - `DotsSerialization` を使って serialization/deserialization を行う実装。

そこそこ構造が複雑で全体の流れを把握することは難しいが、`Chunk` 単位での読み書きが行われていることは簡単に確認できる（`WriteChunks` メソッド）。

### ビジュアルノベルや脱出ゲームではメリットは無いのか？

実のところ、ECS ではデータの独立性が高くなることから、データの処理の流れが把握しにくくなるという弱点がある。それは致命的な弱点ではないものの、OOP と比較すれば、やはり破綻しやすい。ECS でも設計を丁寧に行えば良いとは思われるが、OOP の方がやりやすいことをわざわざ ECS にするメリットはあるだろうか？ また、下手をすれば ECS でパフォーマンスが低下する可能性さえある。となれば、ECS に全くメリットは無いのではないか？

ビジュアルノベルや脱出ゲームは、このシチュエーションに最も嵌まりやすいジャンルであると想像する。

### Prefab の扱いは？

`Baker` の `GetEntity` を使用することで prefab が baking 対象として登録される、という理解で良い。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/baking-prefabs.html
