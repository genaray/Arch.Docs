---
description: >-
  While your servants scurry around you and fulfill their tasks, you suddenly
  ask yourself... how do you actually make these processes more efficient?
---

# Optimizations

Look at how you've worked your way up. There you are as a manager, dealing with the question of how to make everything more efficient and perform better. Come on, let's discuss this in more detail!

## Clojure

The easiest way is to transfer the value directly to the [query](query.md). This is called Clojure. It is simple, but not particularly efficient. For most purposes, however, it will suffice.

<pre class="language-csharp"><code class="lang-csharp"><a data-footnote-ref href="#user-content-fn-1">var deltaTime = 10.0f;</a>
World.Query(in queryDesc, (ref Position pos, ref Velocity vel) => {
    pos.X += vel.X * deltaTime;
    pos.Y += vel.Y * deltaTime;
});
</code></pre>

{% hint style="warning" %}
Allocates the passed value on the heap with each call. Produces garbage that could be avoided.
{% endhint %}

Alternatively, you can cache the `static` value, which avoids the allocation. This is not possible everywhere, but where it works, it works wonderfully.

<pre class="language-csharp"><code class="lang-csharp">private static float _deltaTime;
World.Query(in queryDesc, <a data-footnote-ref href="#user-content-fn-2">static</a> (ref Position pos, ref Velocity vel) => {
    pos.X += vel.X * _deltaTime;
    pos.Y += vel.Y * _deltaTime;
});
</code></pre>

### Inline query

Another way is to pass on the values using a Struct. For this we can use the `InlineQuery` and the `IForEach` interface.

```csharp
private struct VelocityUpdate: IForEach<Position, Velocity>
{
    public float DeltaTime;
    
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void Update(ref Position pos, ref Velocity vel)
    {
        pos.X += vel.X * DeltaTime;
        pos.Y += vel.Y * DeltaTime;
    }
}

var velUpdate = new VelocityUpdate{ DeltaTime = 10.0f; };
World.InlineQuery<VelocityUpdate, Position, Velocity>(in queryDesc, ref velUpdate);
```

{% hint style="success" %}
The struct prevents the value from being associated. This method is fast and efficient, produces no garbage!
{% endhint %}

### Custom loop

That's not enough for you? Well, then we have one last ace up our sleeve. Drink this potion, it will enable you to write queries yourself!

You can now iterate archetypes and chunks yourself, which removes any abstraction. This in turn allows you to pass values and logic however you want.

```csharp
var queryDesc = new QueryDescription().WithAll<Position, Velocity>();
var query = World.Query(in queryDesc);

foreach (ref var chunk in query)
{
    var positions = chunk.GetSpan<Position>();
    var velocities = chunk.GetSpan<Velocity>();
    
    for (var i = 0; i < chunk.Size; i++)
    {
        ref var pos = ref positions[i];
        ref var vel = ref velocities[i];
        
        pos.X += vel.X * DeltaTime;
        pos.Y += velue.Y * DeltaTime;
    }
}
```

{% hint style="success" %}
Fast and efficient. It doesn't really get more efficient than that! It's more boilerplate code, but it's really useful here and there. By the way, if that's not enough for you, have you looked at the source generators?
{% endhint %}

[^1]: Or, of course, as an attribute somewhere.

[^2]: This is optional, but quite nice. It informs the compiler that only static values from outside are allowed in the lambda.
