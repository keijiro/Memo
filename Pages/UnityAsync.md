# Unity Async Programming

Unity 6 の時点でかなり混乱しているが、Unity 6 が出た以上、ここから先、当面は状況に変化が無いだろう、と言うことで、どう取り組むか腰を据えて判断することができる。

### Task vs ValueTask vs Awaitable

公式ドキュメントに対比と使用シチュエーションが並べられているので、まずはこれで判断すること。

https://docs.unity3d.com/6000.0/Documentation/Manual/async-awaitable-introduction.html

### Awaitable as Coroutine

返値無しの `Awaitable` は `IEnumerator` を継承しており、Coroutine として扱えるようなダミーの処理も用意されている。

https://github.com/Unity-Technologies/UnityCsReference/blob/0a1cdaee3d8d8c3a73f80c85a54861538e8db19a/Runtime/Export/Scripting/Awaitable.cs#L409

そこで、次のような書き方もできる。

```csharp
async Awaitable DoSomethingAsync() { ... }

async Awaitable Start()
{
    StartCoroutine(DoSomethingAsync()); // <--
}
```

Coroutine の開始を明示しつつ "call is not awaited" 警告を回避することができる……が、正直、良いプラクティスとは思えない。従来の Coroutine とは異なるものを Coroutine として見せかけるだけで、実質的な処理を伴っていない。最大の問題として、`StartCoroutine` から帰ってきた `Coroutine` に対して何ら処理を行えないというものがある。具体的には `StopCoroutine` を行うことができないし、しかもそれに対してエラーも警告も発されない。

自前で `ForgetAwaitable(IEnumerator e)` のようなダミー関数を用意した方が良いかもしれない。
