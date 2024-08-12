---
description: Now you have taken courage and set off into the big wide world.
cover: ../.gitbook/assets/OMfGL3xXYutK0_jJQ5T6r.png
coverY: 0
---

# World

The world is a great place, isn't it? Huge, beautiful and full of life. There is so much to discover, let's create one of our own.

The **world stores all its** [**entities**](entity.md), it contains methods to create, destroy and query them and handles all the internal mechanics. Therefore it is the most important class, you will use the world heavily. Time to play God.

## The Big Bang

So how does it work with creating worlds? Well, easier than you think...

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

## Prerequisites for life

Now we have a world. A world with all the prerequisites for creating life, changing it and destroying it.

The world has the rudimentary methods on which everything in Arch is based. If you only want to work with them, this is possible and offers one less level of abstraction.&#x20;

{% hint style="info" %}
As an alternative, you can also work ONLY with the world. Without abstraction of entities and co. This principle is called `PURE_ECS` and will be looked at later. The following documentation refers further to the abstracted entity API.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Lifecycle Management</strong></td><td>Creating and destroying Entities. </td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Structural Changes</strong></td><td>Adding/Removing components on Entities</td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Modification</strong> </td><td>Accessing and modifying components on Entities.</td><td><a href="entity.md">entity.md</a></td></tr><tr><td><strong>Enumeration</strong></td><td>Enumerating/Filtering/Matching Entities.</td><td><a href="query.md">query.md</a></td></tr><tr><td><strong>Bulk/Batch Operations</strong></td><td>Executing operations on Entities in Bulk/Batches.</td><td></td></tr><tr><td><strong>Lowlevel</strong></td><td>Working with Archetypes, Chunks and Entities directly without any wrappers. </td><td></td></tr><tr><td><strong>Multithreading</strong></td><td>Acessing the JobScheduler to run querys or your own Jobs in parallel. </td><td></td></tr><tr><td><strong>And much more...</strong></td><td>Working with Events, the CommandBuffer or several other features directly!</td><td></td></tr></tbody></table>

We'll go into more detail later.  But now enough talk, go on. Something is lurking behind the next bush.
