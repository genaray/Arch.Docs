---
description: Archetypes & Chunks, the underlying foundation and storage structure.
---

# Archetypes & Chunks

As you look at your entities, you realise that many of them are similar. They even have an **identical structure**.

And this is exactly where **archetypes** come into play. An **archetype** is something **like a table** in a database; it c**ontains all** [**entities**](entity.md) **with exactly the same component structure**. They are part of the [**World**](world.md) and each new combination of entities is stored in a new archetype.

You don't need to worry about creating them, it all happens on its own under the bonnet.

## Archetype

So what have we learnt? **The structure** of an entity **determines** its **archetype**!

<pre class="language-csharp"><code class="lang-csharp"><a data-footnote-ref href="#user-content-fn-1">var dwarf = world.Create(new Dwarf(), new Position(), new Velocity());</a>
<a data-footnote-ref href="#user-content-fn-2">var elf = world.Create(new Elf(), new Position(), new Velocity());</a>
</code></pre>

{% hint style="info" %}
All elves with the same structure are therefore in one archetype. And all dwarves with the same structure in another.
{% endhint %}

Let's look at another example.

```csharp
var dwarf = world.Create(new Dwarf(), new Position(), new Velocity());
var miningDwarf = world.Create(new Dwarf(), new Position(), new Velocity(), new Pickaxe());
```

{% hint style="success" %}
Even though they are both dwarves, they are still in two different archetypes as their structure is different.&#x20;
{% endhint %}

And now all entities with the same structure cuddle up very close to each other in their archetype, nice... right?

## Chunk

So now you've learnt what an **archetype** is. But what is this **chunk** that was mentioned earlier?

Well, while **archetypes do the grouping**, **chunks** are the place where the **entities** and their **components** are **stored in memory**. This also happens under the bonnet and you don't notice much of it.

Each chunk has exactly **16KB of memory** which is used entirely for entities and their components. This amount of data fits perfectly into an **L1 cache** and is therefore loaded incredibly quickly and efficiently when the entities and components are enumerated.&#x20;

Another advantage is that **each archetype has N chunks**, so new chunks can be quickly **allocated** for more entities or existing chunks can be **trimmed** to free up memory. The best of the best to keep your world running!

{% hint style="info" %}
The size of the chunks adapts dynamically, the **default** base size is **16KB** but the archetype decides based on the composition of the entities whether larger chunks might make sense. **If too few entities fit into a chunk, all chunks are enlarged to the next largest multiple of the base size.** The base chunk size and the minimum number of entities that fit into each chunk can be adjusted when creating the [`World`](world.md).
{% endhint %}

[^1]: Creates a dwarf entity which is in an archetype containing all entities with exactly the same components.

[^2]: Creates an elf entity which is in a different archetype where all entities have exactly the same components.
