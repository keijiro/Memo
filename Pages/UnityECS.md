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
