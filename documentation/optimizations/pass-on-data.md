---
description: Passing on data, a way to use data from outside in your queries
---

# Pass on data

In many cases, it is simply not enough to work with data on the entities alone. Sometimes you also need data from outside... but how do you even do that?

## Closure

The easiest way is to transfer the value directly to the [query](../query.md). This is called **Closure**. It is simple, but not particularly efficient. For most purposes, however, it will suffice.

<pre class="language-csharp"><code class="lang-csharp"><a data-footnote-ref href="#user-content-fn-1">var intruderPosition = DetectIntruder();</a>
World.Query(in queryDesc, (ref Position pos, ref Velocity vel) => {
    <a data-footnote-ref href="#user-content-fn-2">MoveTowards(ref pos, ref vel, intruderPosition);</a>
});
</code></pre>

{% hint style="warning" %}
Allocates the passed value on the heap with each call. **Produces garbage** that could be avoided.
{% endhint %}

Alternatively, you can cache the `static` value, which **avoids** the **allocation**. This is not possible everywhere, but where it works, it works wonderfully.

<pre class="language-csharp"><code class="lang-csharp">var intruderPosition = DetectIntruder();
World.Query(in queryDesc, <a data-footnote-ref href="#user-content-fn-3">static</a> (ref Position pos, ref Velocity vel) => {
    MoveTowards(ref pos, ref vel, intruderPosition);
});
</code></pre>

And already all our entities are moving in the direction of the intruder, brilliant... right?

## Inline query

Another way is to pass on the values using a Struct. For this we can use the `InlineQuery` and the `IForEach` interface.

```csharp
private struct VelocityUpdate: IForEach<Position, Velocity>
{
    public Position IntruderPosition;
    
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void Update(ref Position pos, ref Velocity vel)
    {
        MoveTowards(ref pos, ref vel, IntruderPosition);
    }
}

var velUpdate = new VelocityUpdate{ IntruderPosition = DetectIntruder(); };
World.InlineQuery<VelocityUpdate, Position, Velocity>(in queryDesc, ref velUpdate);
```

{% hint style="success" %}
The struct prevents the value from being associated. This method is fast and efficient, produces no garbage!
{% endhint %}

Now our entities are also moving in the direction of the intruder... but more efficiently. Especially good if we have a lot of entities. It's also quite nice, isn't it? But there is another variant.

## Custom loop

That's not enough for you? Well, then we have one last ace up our sleeve. You can also write queries yourself!

You can now iterate **archetypes** and **chunks** yourself, which removes any abstraction. This in turn allows you to pass values and logic however you want.

<pre class="language-csharp"><code class="lang-csharp">var intruderPosition = DetectIntruder();
var queryDesc = new QueryDescription().WithAll&#x3C;Position, Velocity>();
<a data-footnote-ref href="#user-content-fn-4">var query = World.Query(in queryDesc);</a>

foreach (ref var chunk in query)
{
    <a data-footnote-ref href="#user-content-fn-5">var positions = chunk.GetSpan&#x3C;Position>();</a>
    var velocities = chunk.GetSpan&#x3C;Velocity>();
    
    <a data-footnote-ref href="#user-content-fn-6">foreach (var entityIndex in chunk)</a>
    {
        ref var pos = ref positions[entityIndex];
        ref var vel = ref velocities[entityIndex];
        
        MoveTowards(ref pos, ref vel, intruderPosition);
    }
}
</code></pre>

{% hint style="success" %}
Fast and efficient. It doesn't really get more efficient than that! It's more boilerplate code, but it's really useful here and there. By the way, if that's not enough for you, have you looked at the [source generators](../../extensions/page-3/arch.system.sourcegenerator.md)?
{% endhint %}

And now you can move even more entities efficiently, perhaps even hundreds of thousands? Or even millions? Who knows, time to let them march!

[^1]: Can of course also be stored somewhere as an attribute. A fictitious method that returns a position.

[^2]: Also fictitious, symbolises that the entity moves to the specified position. Uses the passed `intruderPosition.`

[^3]: This is optional, but quite nice. It informs the compiler that only static values from outside are allowed in the lambda.

[^4]: Returns a [`Query`](../query.md) that allows us to manually iterate over all [entities](../entity.md) that are addressed by it.

[^5]: Returns the underlying component arrays as spans. We can iterate these ourselves to execute logic.

[^6]: Enumerates over all [Entities](../entity.md) in this chunk to acess their components.
