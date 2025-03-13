---
description: The World, the place where all Entities live.
---

# World

The **world stores all its** [**entities**](entity.md), it contains methods to create, destroy and query them and handles all the internal mechanics. Therefore it is the most important class, you will use the world heavily. Time to play God.

## Lifecycle

```csharp
var world = World.Create();  // Creating a world full of life
world.TrimExcess();          // Frees unused memory
world.Dispose();             // Clearing the world like God in the First Testament
World.Destroy(world);        // Doomsday
```

{% hint style="info" %}
Multiple worlds can be used in parallel, each instance and its entities are completely encapsulated from other worlds
{% endhint %}

Best of all, you can create any number of worlds with almost infinite entities. You rarely have so much life and freedom. To be precise,`2,147,483,647` worlds with `2,147,483,647` entities each are possible.&#x20;

## Customize

You can also customize the world a little to suit your needs, for example the initial capacity or how big the [`chunks`](archetypes-and-chunks.md) should be:

<pre class="language-csharp"><code class="lang-csharp">var customizedWorld = World.Create(
<strong>    chunkSizeInBytes: 16_382,              // The base size of each chunk
</strong><strong>    minimumAmountOfEntitiesPerChunk: 100,  // The base amount of entities per chunk
</strong><strong>    archetypeCapacity: 8,                  // The initial size of the archetype array
</strong><strong>    entityCapacity: 64                     // The initial amount of entities
</strong>);
</code></pre>

{% hint style="info" %}
Adjusting this is not absolutely necessary, but can be quite helpful. These are always initial values that Arch uses as a guide. It may well be that Arch deviates upwards in some cases, especially with the `chunkSizeInBytes` or the `minimumAmountOfEntitiesPerChunk`.
{% endhint %}

## Good to know

Now we have a world. A world with all the prerequisites for creating life, changing it and destroying it.

The world has the rudimentary methods on which everything in Arch is based. If you only want to work with them, this is possible and offers one less level of abstraction.&#x20;

{% hint style="info" %}
As an alternative, you can also work ONLY with the world. Without abstraction of entities and co. This principle is called [`PURE_ECS`](optimizations/pure_ecs.md) and will be looked at later. The following documentation refers further to the abstracted entity API.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Lifecycle Management</strong></td><td>Creating and destroying Entities. </td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Structural Changes</strong></td><td>Adding/Removing components on Entities</td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Modification</strong> </td><td>Accessing and modifying components on Entities.</td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Enumeration</strong></td><td>Enumerating/Filtering/Matching Entities.</td><td><a href="query.md">query.md</a></td></tr><tr><td><strong>Bulk/Batch Operations</strong></td><td>Executing operations on Entities in Bulk/Batches.</td><td><a href="optimizations/batch-and-bulk.md">batch-and-bulk.md</a></td></tr><tr><td><strong>Lowlevel</strong></td><td>Working with Archetypes, Chunks and Entities directly without any wrappers. </td><td><a href="archetypes-and-chunks.md">archetypes-and-chunks.md</a></td></tr><tr><td><strong>Multithreading</strong></td><td>Acessing the JobScheduler to run querys or your own Jobs in parallel. </td><td><a href="optimizations/multithreading.md">multithreading.md</a></td></tr><tr><td><strong>And much more...</strong></td><td>Working with Events, the CommandBuffer or several other features directly!</td><td></td></tr></tbody></table>

We'll go into more detail later.  But now enough talk, go on. Something is lurking behind the next bush.
