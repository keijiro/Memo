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

### Prefab の扱いは？

`Baker` の `GetEntity` を使用することで prefab が baking 対象として登録される、という理解で良い。

https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/baking-prefabs.html
