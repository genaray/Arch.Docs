---
description: >-
  Structural changes, what you need to consider to avoid shooting yourself in
  the foot.
---

# Structural changes

When making changes to the structure of an [`Entity`](../documentation/entity.md), there are also a few things to consider. In most cases, you can simply edit an entity using `Add` and `Remove`.&#x20;

```csharp
var entity = World.Create<Dwarf, Position, Velocity>();
entity.Add<Pickaxe>(); 
```

{% hint style="success" %}
The entity gets a new component and does not have it yet, that fits.
{% endhint %}

But sometimes you need to take a closer look.&#x20;

```csharp
var entity = GetRandomEntity();  // Returns an entirely random entity
entity.Add<Pickaxe>();           // What if it already has that one?
entity.Remove<Helmet>();         // What if it does NOT have this one? 
```

{% hint style="danger" %}
Add and Remove do not check whether the [`Entity`](../documentation/entity.md) already has these components. In this case, the entity could already have these components and it would lead to a corrupted memory.
{% endhint %}

It is therefore recommended that you always check beforehand if you cannot guarantee that an entity has or does not have a component.

```csharp
var entity = GetRandomEntity();  // Returns an entirely random entity
if(!entity.Has<Pickaxe>() entity.Add<Pickaxe>();           
if(entity.Has<Helmet>()) entity.Remove<Helmet>();         
```

## Why is that?

<details>

<summary><strong>The reason</strong></summary>

You're probably wondering why we don't just install a few security checks to prevent this kind of behavior? Good question!&#x20;

{% hint style="warning" %}
Arch's mantra is “Pay what you use” to ensure maximum performance. Arch does nothing in the background to slow down your code, so you need to know what you are doing and sometimes check yourself.&#x20;
{% endhint %}

This has pros and cons, one advantage is clean code and performance. In the future Arch will get optional security checks.

</details>

