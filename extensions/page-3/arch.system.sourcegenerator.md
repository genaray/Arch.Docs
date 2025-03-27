---
description: Arch.System.SourceGenerator, automatically generates queries for you.
---

# Arch.System.SourceGenerator

The `Arch.System.SourceGenerator` package provides some code generation utils. With them, queries can be written virtually by themselves which saves some boilerplate code.

**Query** methods can be generated **in** [**all classes**](#user-content-fn-1)[^1] **as long as they are partial**. However it makes most sense in the [`BaseSystem`](arch.system.md). The attributes can be used to meaningfully describe what query to generate, and the query will always call the annotated method. And the best, they can even be called manually!

## Example

Let's take a look at the whole thing and what is possible with it.

<pre class="language-csharp"><code class="lang-csharp">public partial class MovementSystem : BaseSystem&#x3C;World, <a data-footnote-ref href="#user-content-fn-2">float</a>>
{

    public MovementSystem(World world) : base(world) {}
    
    <a data-footnote-ref href="#user-content-fn-3">[Query] </a>
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void MoveEntity(<a data-footnote-ref href="#user-content-fn-4">[Data]</a> in float time, <a data-footnote-ref href="#user-content-fn-5">ref Position pos, in Velocity vel</a>)
    {
        pos.X += time * vel.X;
        pos.Y += time * vel.Y;
    }
    
    [Query]
    <a data-footnote-ref href="#user-content-fn-6">[All&#x3C;Player, Mob, Particle>, Any&#x3C;Moving, Idle>, None&#x3C;Alive>] </a>
    public void StopDeadEntities(ref Velocity vel)
    {
        vel.X = 0;
        vel.Y = 0;
    }
}
</code></pre>

{% hint style="info" %}
Again, there are attributes without generics that accept types. In addition, you can also pass an `entity` in the method signature of a `[Query]` to refer to the current entity. In addition to `ref`, you can also use `in` or `out` or omit the modifier altogether.
{% endhint %}

Each method marked with `[Query]` is extended to a `Query` by the source generation and called once for each `Entity` that matches it. The method signature and additional annotations act as a filter. This means that the `MoveEntity` method from above is called for each `Entity` with `Position` and `Velocity`. We only write the method with which we process each `Entity` and what we need... and `Arch.System.SourceGenerator` takes care of the rest.&#x20;

A method with the same name and query is generated for each of these selected methods. This query method then calls our selected method for each `Entity`. E.g. `MoveEntity` which is getting called for each fitting `Entity` and `MoveEntityQuery`, the generated method that runs the query and calls `MoveEntity` for each `Entity`. This happens for every method within a class that has been marked with `Query`. So also for `StopDeadEntities`.&#x20;

In addition, if `BaseSystem.Update` has not been implemented itself, a `BaseSystem.Update` is generated which calls all `Queries` according to their sequence in the system. So all you have to do is call `BaseSystem.Update()` or the generated query directly.

```csharp
var world = World.Create();
var movementSystem = new MovementSystem(world);

myMovementSystem.Update(deltaTime);            // Calls both Querys in order automatically
myMovementSystem.MoveEntityQuery(world);       // Manually calling the querys
myMovementSystem.StopDeadEntitiesQuery(world);
```

It can be that easy, I bet that blew your mind, didn't it?

## Overriding Update

If you use the generator as above then an `Update` method is automatically generated for you which calls all queries one after the other. However, if you want to have control over the update method yourself, you can do it this way:

```csharp
public partial class MovementSystem : BaseSystem<World, GameTime>
{
    private readonly QueryDescription _customQuery = new QueryDescription().WithAll<Position, Velocity>();
    public DebugSystem(World world) : base(world) { }

    // Adding this will give you manual control
    public override void Update(in GameTime t)
    {
        World.Query(in _customQuery, (in Entity entity) => /** LOGIC **/ ));
        MoveEntityQuery(World);  // Call source generated query, which calls the MoveEntity method
    }

    [Query]
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void MoveEntity([Data] in float time, ref Position pos, ref Velocity vel)
    {
        pos.X += time * vel.X;
        pos.Y += time * vel.Y;
    }
}
```

{% hint style="info" %}
If you provide the update method yourself, you must also ensure that your generated queries (in the update method) are called!
{% endhint %}

## Generating Queries in custom classes

However, you do not necessarily have to use the `BaseSystem` with the SourceGenerator. Instead, you can have queries generated wherever you want!

<pre class="language-csharp"><code class="lang-csharp"><a data-footnote-ref href="#user-content-fn-7">public partial class MyQueries</a>{

    [Query]
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void MoveEntities([Data] in float time, ref Position pos, ref Velocity vel){
        pos.X += time * vel.X;
        pos.Y += time * vel.Y;
    }
    
    [Query]
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public <a data-footnote-ref href="#user-content-fn-8">static</a> void DamageEntities(ref Hitted hit, ref Health health){
        health.value -= hit.value;
    }
}
</code></pre>

{% hint style="info" %}
The same works for static classes, whose performance is better. The advantage is that no class instance is needed.
{% endhint %}

## Multithreading

Arch supports parallel queries directly out of the box. The source generator also does, with a simple attribute you can ensure that the query is executed on multiple threads:

```csharp
[Query(Parallel = true)]  // Method is being called by Archs multithreading. 
[MethodImpl(MethodImplOptions.AggressiveInlining)]
public static void MoveEntities([Data] in float time, ref Position pos, ref Velocity vel){
    pos.X += time * vel.X;
    pos.Y += time * vel.Y;
}
```

[^1]: Also static ones, without a `BaseSystem` inheritance.&#x20;

[^2]: A value that we pass into the system with `Update` and which we can access with `[Data]` in the query.

[^3]: Marks this method so that a query is generated from this method and its signature.

[^4]: Specifies that this value is entered into the query from outside in order to process it further here.

[^5]: The components of the individual entity that we access and edit.

[^6]: A filter so that we only process certain entities.

[^7]: Also works if this class is `static`.&#x20;

[^8]: Generating static methods also works!&#x20;
