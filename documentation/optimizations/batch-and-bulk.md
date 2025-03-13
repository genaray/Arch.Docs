---
description: Batch and Bulk, executing batched commands on all your entities.
---

# Batch and Bulk

So... you don't want to **edit** all your [**entities**](../entity.md) individually, but **all at once**? How good that I know what makes you tick! In some cases, it is more efficient to carry out operations not on each entity individually, but on all of them together. This is what this chapter is about.

## Reserve

Arch offers you the possibility to reserve memory for your entities in advance. This speeds up the creation of entities, as the memory already exists and does not have to be allocated later.

```csharp
world.EnsureCapacity<Dwarf, Position, Velocity>(1_000_000);
```

{% hint style="info" %}
The `world.EnsureCapacity` method reserves memory for [**entities**](../entity.md) of a specific component structure. The [`archetype`](../archetypes-and-chunks.md#order-in-chaos) that contains entities of this specific structure is therefore enlarged so that there is enough space for them.
{% endhint %}

However, these have not yet been created, the space has only been reserved. This still has to happen, the moment does not matter, the reserved memory does not expire. All in all, it looks like this:

<pre class="language-csharp"><code class="lang-csharp">world.EnsureCapacity&#x3C;Dwarf, Position, Velocity>(1_000_000);
for(var index = 0; index &#x3C; 1_000_000; index++){
<strong>    var dwarf = world.Create(signature);
</strong>}
</code></pre>

{% hint style="info" %}
So space is created for around one million dwarves and this space is then filled by creating the entities through `world.create`. This is fast and efficient, it only needs to allocate memory once. Of course theres also an [`Non-Generic API`](../utilities/non-generic-api.md)which accepts an `Signature` instead.
{% endhint %}

The `world.EnsureCapacity` method works like an array or list. This means that it does not allocate memory for n additional elements, but ensures that there is space for a total of n elements.

## Create

If you not only want to reserve memory, but also create many entities at the same time. Then the `world.Create` overload is a good choice.

```csharp
world.Create(1_000_000, new Dwarf(), RandomPosition(), RandomVelocity());
```

{% hint style="info" %}
Each entity created will have exactly the same components (and exactly the same values). There is of course a [`Non-Generic API`](../utilities/non-generic-api.md) that takes a `Signature` instead and even returns a list of `Entity`!
{% endhint %}

## Change

But what if you want to change all entities at once? That works too, of course, and has the same advantages.

<pre class="language-csharp"><code class="lang-csharp">// Giving our dwarfs equipment
var allDwarfs = new QueryDescription().WithAll&#x3C;Dwarf>().WithNone&#x3C;Axe, Armor, Boots>();
<a data-footnote-ref href="#user-content-fn-1">world.Add(in allDwarfs, new Axe(state = State.Broken), new Armor(), new Boots());</a>

// Repairing the axe of all ours dwarfs 
var dwarfsWithAxe = new QueryDescription().WithAll&#x3C;Dwarf, Axe>();
world.Set(in dwarfsWithAxe, new Axe(state = State.Repaired));

// Deciding that our dwarfs are too heavy and they dont need any armor
var dwarfsWithArmor = new QueryDescription().WithAll&#x3C;Armor>();
world.Remove&#x3C;Armor>(in dwarfsWithArmor);

// Destroying all dwarfs
<a data-footnote-ref href="#user-content-fn-2">world.Destroy(in dwarfsWithAxe);</a>
</code></pre>

{% hint style="info" %}
Each of these operations uses generics and is available in many variations and overloads. Up to **25 parameters** are accepted, but do not have to be passed, in which case the **default values** are used.
{% endhint %}

These perform their operations on all [**entities**](../entity.md) that match the [**query**](../query.md). The entities are not processed individually but as a set, making this incredibly fast and efficient.&#x20;

[^1]: `Axe`, `Armor`and`Boots` are added for all [**entities**](../entity.md) that match this [**query**](../query.md).

[^2]: Deletes all entities of this query, but does not release the memory that is no longer occupied. This means that the capacity is retained and can then be filled again. If this is not desired, then `world.TrimExcess` should be called.
