# Unity ECS

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

### Prefab の扱いは？

`Baker` の `GetEntity` を使用することで prefab が baking 対象として登録される、という理解で良い。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/baking-prefabs.html
