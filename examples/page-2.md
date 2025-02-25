---
description: >-
  Entities in Query, how to handle entities in queries correctly and what to
  bear in mind.
---

# Entities in Query

We need to have another serious talk about what you can and should and should not do in [`Querys`](../documentation/query.md)! So sit down and listen!

In principle, we recommend the use of a [`CommandBuffer`](../documentation/utilities/commandbuffer.md) **when** it comes to **creating** or **deleting** entities in a **query** or changing their structure. Or to simply make these changes outside a query. **Changes to component values are never a problem.**

## Creating entities in Querys

Creating entities in `Queries` is harmless for the time being, nothing can go wrong.

```csharp
world.Query(queryDesc, (ref DwarfSpawner spawner) => {
    
    if(!spawner.CanSpawnDwarf()) return;
    world.Create<Dwarf, Position, Velocity>();
});
```

{% hint style="success" %}
The entities are created appropriately, everything is great. Arch iterates from back to front, new entities are appended at the end and therefore not reached during the query.
{% endhint %}

## Deleting entities in Querys

This is where it gets interesting and fun. Let's take a look at this case.

```csharp
world.Query(queryDesc, (in Entity entity, ref Dwarf dwarf) => {
    
    if(!CanDeleteDwarf()) return;  // A random chance
    world.Destroy(RandomDwarf());
});
```

{% hint style="danger" %}
This won't work and will lead to issues. We are iterating over all `Dwarfs` and deleting one randomly. When an `Entity` is deleted, the last entity from the [`Archetype`](../documentation/archetypes-and-chunks.md) takes the place of the deleted entity to keep the list without gaps. In this case, it can happen that we iterate over an entity twice because it takes the place of the deleted entity during the iteration
{% endhint %}

In such a case, we should use the [`CommandBuffer`](../documentation/utilities/commandbuffer.md) or maintain our own list of entities, which we then delete ourselves **after** the `Query`. What works, however, is if we have a deterministic order when deleting.

```csharp
world.Query(queryDesc, (in Entity entity, ref Dwarf dwarf) => {
    world.Destroy(entity);
});
```

{% hint style="success" %}
This is fine, we iterate all `Dwarfs` one by one and all of them are being deleted.&#x20;
{% endhint %}

What also works, of course, is deleting entities in a query that we are not currently iterating over.

```csharp
world.Query(queryDesc, (in Entity entity, ref Dwarf dwarf) => {
    world.Destroy(RandomElf());
});
```

{% hint style="success" %}
This also works fine, since we are not deleting any `Entity` from the current iteration.&#x20;
{% endhint %}

