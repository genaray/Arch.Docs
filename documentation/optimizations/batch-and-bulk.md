---
description: >-
  How great would it be if you could execute a command on ALL your creatures...
  oh... you can do that!
---

# Batch and Bulk

So... you don't want to **edit** all your [**entities**](../entity.md) individually, but **all at once**? How good that I know what makes you tick, come here my son, I can still teach you something!

## Mass production

So after your long journey you have finally arrived in the industrial age... Now you want to mass produce entities? Let's take a look at that...

<pre class="language-csharp"><code class="lang-csharp">var signature = <a data-footnote-ref href="#user-content-fn-1">new Signature(typeof(Dwarf), typeof(Position), typeof(Velocity));</a>
world.Reserve(signature, 1_000_000);
</code></pre>

{% hint style="info" %}
The `world.reserve` method reserves memory for [**entities**](../entity.md) of a specific component structure. The [`archetype`](../archetypes-and-chunks.md#order-in-chaos) that contains entities of this specific structure is therefore enlarged so that there is enough space for them.
{% endhint %}

However, these have not yet been created, the space has only been reserved. This still has to happen, the moment does not matter, the reserved memory does not expire. All in all, it looks like this:

<pre class="language-csharp"><code class="lang-csharp">var signature = new Signature(typeof(Dwarf), typeof(Position), typeof(Velocity));
world.Reserve(signature, 1_000_000);
for(var index = 0; index &#x3C; 1_000_000; index++){
<strong>    var dwarf = world.Create(signature);
</strong>}
</code></pre>

{% hint style="info" %}
So space is created for around one million dwarves and this space is then filled by creating the entities through `world.create`. This is fast and efficient, it only needs to allocate memory once.
{% endhint %}

And within a short space of time, you've built up a huge army... and it's incredibly efficient! Whats your... next... step?!

## Mass customisation

A further step of every good dictator, uhh leader, is to adapt his troops for the coming campaigns. Customising each one individually is too much effort, so it's a good thing that there are modern production lines for this!

<pre class="language-csharp"><code class="lang-csharp">// Giving our dwarfs equipment
var allDwarfs = new QueryDescription().WithAll&#x3C;Dwarf>().WithNone&#x3C;Axe, Armor, Boots>();
<a data-footnote-ref href="#user-content-fn-2">world.Add(in allDwarfs, new Axe(state = State.Broken), new Armor(), new Boots());</a>

// Repairing the axe of all ours dwarfs 
var dwarfsWithAxe = new QueryDescription().WithAll&#x3C;Dwarf, Axe>();
world.Set(in dwarfsWithAxe, new Axe(state = State.Repaired));

// Deciding that our dwarfs are too heavy and they dont need any armor
var dwarfsWithArmor = new QueryDescription().WithAll&#x3C;Armor>();
world.Remove&#x3C;Armor>(in dwarfsWithArmor);

// Destroying all dwarfs
<a data-footnote-ref href="#user-content-fn-3">world.Destroy(in dwarfsWithAxe);</a>
</code></pre>

{% hint style="info" %}
Each of these operations uses generics and is available in many variations and overloads. Up to **25 parameters** are accepted, but do not have to be passed, in which case the **default values** are used.
{% endhint %}

These perform their operations on all [**entities**](../entity.md) that match the [**query**](../query.md). The entities are not processed individually but as a set, making this incredibly fast and efficient. Perfect for our huge army!

[^1]: The signature defines the component structure of an entity, i.e. it is effectively the structure. This notation is part of the [non-generic API](../utilities/non-generic-api.md). Alternatively, `Component<T0...>.Signature` can also be used.

[^2]: `Axe`, `Armor`and`Boots` are added for all [**entities**](../entity.md) that match this [**query**](../query.md).

[^3]: Deletes all entities of this query, but does not release the memory that is no longer occupied. This means that the capacity is retained and can then be filled again. If this is not desired, then `world.TrimExcess` should be called.
