---
description: If only there was a way to organize your queries... wait... there is!
---

# Arch.System

You write query after query all the time to order your subordinates around... it's a pure mess, isn't it? Then just use `Arch.System`? It's quite simple... With `Arch.System` you can organize your queries in `Systems` and call and reuse them at will.

<pre class="language-csharp"><code class="lang-csharp">public class MovementSystem : <a data-footnote-ref href="#user-content-fn-1">BaseSystem&#x3C;World, float></a>
{
    private QueryDescription _desc = new QueryDescription().WithAll&#x3C;Position, Velocity>();
    public MovementSystem(World world) : base(world) {}
    
    // Can be called once per frame
    public override void Update(in <a data-footnote-ref href="#user-content-fn-2">float deltaTime</a>)
    {
        // Run query, can also run multiple queries inside the update method
        World.Query(in _desc, (ref Position pos, ref Velocity vel) => {
            pos.X += vel.X;
            pos.Y += vel.Y;
        });  
    }
}
</code></pre>

{% hint style="info" %}
The example above is of course kept simple. You can simply put anything you want to group into such a `System`, e.g. a `System` for everything that moves your creatures... another `System` that contains everything that causes your creatures to suffer damage and heals them etc. There is no limit to your imagination!
{% endhint %}

Now we already have a `MovementSystem`, a `System` that groups everything that makes our [`Entities`](../../documentation/entity.md) move. But how do we integrate this now?&#x20;

```csharp
// Create a world and a group of systems which will be controlled 
var world = World.Create();
var _systems = new Group<float>(
    new MovementSystem(world),  
    new PhysicsSystem(world),
    new DamageSystem(world),
    new WorldGenerationSystem(world),
    new OtherGroup(world)
);

_systems.Initialize();                  // Inits all registered systems
_systems.BeforeUpdate(in deltaTime);    // Calls .BeforeUpdate on all systems ( can be overriden )
_systems.Update(in deltaTime);          // Calls .Update on all systems ( can be overriden )
_systems.AfterUpdate(in deltaTime);     // Calls .AfterUpdate on all System ( can be overriden )
_systems.Dispose();                     // Calls .Dispose on all systems ( can be overriden )
```

{% hint style="info" %}
A `Group<T>` can also contain other `Group<T>`. `BeforeUpdate`, `Update` and `AfterUpdate` should be called at will to update the systems and queries.
{% endhint %}

And we have already divided our `Systems` into `Group<T>`s to bring order into it. What more could you want?

[^1]: `BaseSystem` provides several usefull methods for interacting and structuring systems. `float` is the value that is transmitted to the system. This can be any type.

[^2]: Can be any type based on the generics of this instance.
