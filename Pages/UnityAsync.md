# Unity Async Programming

## My personal policy for using Awaitable

### Don't create unhandled Awaitables

```csharp
async Awaitable DoSomethingAsync() { ... }

void FooMethod()
{
    DoSomethingAsync(); // <- Unhandled Awaitable
}
```

Unhandled Awaitables like the example above are error-prone, because exceptions
thrown inside them are never observed—nothing is shown in the Console. As a
result, errors occurring in async tasks can easily go unnoticed.

This issue can be addressed by providing a `Forget` method like the following:

```csharp
public static class AwaitableExtensions
{
    public static async void Forget(this Awaitable awaitable)
    {
        try
        {
            await awaitable;
        }
        catch (OperationCanceledException)
        {
        }
        catch (Exception e)
        {
            Debug.LogException(e);
        }
    }
}
```

Usage:

```csharp
void FooMethod()
{
    DoSomethingAsync().Forget();
}
```

### Don't use an `async Awaitable` Start coroutine

```csharp
async Awaitable Start()
{
    ...
}
```

This also creates an unhandled Awaitable. It can be easily avoided by changing
the return type to `async void`:

```csharp
async void Start()
{
    ...
}
```
