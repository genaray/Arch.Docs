---
description: The Query, a way to select and iterate entities
---

# Query

A `Query` is a view of the [World](world.md) and **targets a specific set of** [**entities**](entity.md). It only consists of a `QueryDescription`, which specifies which entities with **which structure are being searched for**. You can specify which components an entity should definitely have, which it should perhaps have and which it should not have at all. It is also possible to search only for the exclusive structure of an entity. Let's take a look at this!

## All

A normal iteration targets entities **WITH** specific components. In this example, we want to iterate over all entities that have `Position` and `Velocity` to move them.

<pre class="language-csharp"><code class="lang-csharp">// Creating many different entities
for(var index = 0; index &#x3C; 1_000, index++){
    world.Create(new Dwarf(), new Position(0,0), new Velocity(1,1), new Pickaxe());
    world.Create(new Elf(), new Position(0,0), new Velocity(1,1), new Bow());
    world.Create(new Human(), new Position(0,0), new Velocity(1,1), new Pickaxe(), new Sword());
}

// Iterating over all entities that have Position &#x26; Velocity to make them move. 
<a data-footnote-ref href="#user-content-fn-1">var movementQuery = new QueryDescription().WithAll&#x3C;Position, Velocity>();</a>
world.Query(in movementQuery, (Entity entity, ref Position pos, ref Velocity vel) => {
    pos += vel;
    Console.WriteLine($"Moved: {entity}");
});
</code></pre>

{% hint style="info" %}
The methods of the `QueryDescription` can accept up to 25 generic parameters and can be chained together.  Can also be created without generics by passing [`Signature`](utilities/non-generic-api.md). There multiple different [`Query-Variants`](optimizations/query-techniques.md). More on this later.
{% endhint %}

It doesn't matter whether they are dwarves, elves or humans. As long as they have the **Position & Velocity** components, they move!

## Any

But what if we need more filters to have all entities except some specific ones? For example, everyone with a `Pickaxe` **EXCEPT** `Elves`to mine ores. It's time to make the queries a little more complex.

```csharp
// Targets entities with a pickaxe that are not Elven. So humans and dwarves.
var letDwarfsAndHumansMine = new QueryDescription().WithAll<Pickaxe>().None<Elf>();
world.Query(in movementQuery, (ref Pickaxe pickaxe) => {
    var rock = FindNextRock();
    MineSomeOres(pickaxe, rock);
});
```

{% hint style="info" %}
You do not necessarily have to pass the [entity](entity.md) in a `Query`.
{% endhint %}

## None

But there is another important filter. Imagine we are attacked and we now want to call all entities with a weapon to the defense. So we need everyone who has **ANY** weapon.

```csharp
// Targets entities with either a bow or sword. So elves and humans. 
var letElvesAndHumansPatrol = new QueryDescription().WithAny<Bow, Sword>();
world.Query(in movementQuery, (Entity entity) => {
    
    ref var bow = entity.TryGet<Bow>(out var hasBow);
    ref var sword = entity.TryGet<Sword>(out var hasSword);

    if(EnemyNearby()){
        if(hasBow) bow.Attack();
        if(hasSword) sword.Attack();
    }
});
```

## Exclusive

There is another case of filters. Sometimes we want to search for entities that have an exclusive set of components. No more and no less than that. In our example, a new type of dwarf that no longer wears pickaxes.

```csharp
// New breed of dwarfs, too weak to wear pickaxes
for(var index = 0; index < 1_000, index++){
    world.Create(new Dwarf(), new Position(0,0), new Velocity(1,1));
}
```

You want to give them an exclusive task. But how do we tell them apart from the rest? With one of those?

```csharp
var firstQueryDesc = new QueryDescription().WithAll<Dwarf, Position, Velocity>();
var secondQueryDesc = new QueryDescription().WithAll<Dwarf, Position, Velocity>().None<Pickaxe>();
```

{% hint style="danger" %}
No. Both `QueryDescriptions` would also target dwarfs that have additional components (e.g. \[`Dwarf, Position, Velocity, Bow`]). We do not want that.
{% endhint %}

Instead, we only want to target this one type of dwarf. No more or less components on them.

```csharp
// Only targeting entities with exact those components, no more or less.
var makeWeakDwarfsExile = new QueryDescription().WithExclusive<Dwarf, Position, Velocity>();
world.Query(in makeWeakDwarfsExile, (Entity entity) => {
    Exile(entity);
});
```

Next, perhaps we should take a look at what's under the hood of Arch.

[^1]: This is also possible, but it would be better if the `QueryDescription` was not created before each query, but only once.
