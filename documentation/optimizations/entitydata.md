---
description: EntityData, a way to access meta data of entities directly.
---

# EntityData

With EntityData there is now also a way to access the metadata of entities directly. This saves repeated lookups and allows you to optimize your code.

## Example

```csharp
var entity = world.Create(new Dwarf(), new Pickaxe());
ref var data = ref world.GetEntityData(entity);

// Less lookups, faster then doing it the traditional way 
ref var pickaxe = ref data.Get<Pickaxe>();
pickaxe.HP = 100;
data.Set<Pickaxe>(new Pickaxe());

// Access archetype & chunk 
var archetype = data.Archetype;
ref var chunk = ref data.Chunk; 
```

{% hint style="warning" %}
This means that only **Get** and **Set** operations can be executed, **no Add and Remove**!
{% endhint %}
