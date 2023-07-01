# Unity ECS

## Source generators

Source generator のソースコードは `Unity.Entities/SourceGenerators/Source~/SystemGenerator` 下に格納されている。

以下、README から転載。

Source generator は以下の機能を提供するために用いられる。

- Entities.ForEach
- Job.WithCode
- Aspects
- Idiomatic C# `foreach`
- SystemAPI
- IJobEntity

Generator は type, method, partial type の追加を行うことでこれを実現する。特定の条件下においては、既存コードに対するパッチングも Cloner IL ポストプロセスを使って行われる。
