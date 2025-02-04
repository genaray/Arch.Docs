---
description: >-
  You make your way through the thicket and suddenly there's an entity... but
  what is it?
cover: ../.gitbook/assets/wJcs9lQWunu55aaeUEwKq.png
coverY: 55
---

# Entity

In an [entity component system (ECS)](concepts.md), an entity is simply a **unique ID** tag. It represents **an object** in the game or application, but has no logic or data itself. The data and behavior come from **components** that are attached to this entity.

An `Entity` is nothing more than a simple `record struct Entity{ int ID, int WorldId }`. Either you work directly on this entity and its methods, or you simply use [`PURE_ECS`](optimizations/pure\_ecs.md). More on this later.

{% hint style="info" %}
Its components can be **anything**, **primitive data types**, **structs** and even **classes**! YOU determine which components and attributes an entity receives, there are **no restrictions** here!
{% endhint %}

## Creating life

I'm not a dark mage, but I can tell you a little about how to create life. It's actually surprisingly simple... Let's take a look!

<pre class="language-csharp"><code class="lang-csharp">var world = World.Create(); // Must exist somewhere

// A dwarf with a string, position and velocity component was born and killed
<strong>var dwarf = world.Create(new Dwarf("Gimli"), new Position(0,0), new Velocity());
</strong><strong>if(dwarf.IsAlive()) world.Destroy(dwarf);
</strong></code></pre>

{% hint style="info" %}
Components may vary and this example uses generics, if you don't feel like using generics, there are still [plenty of APIs without them](utilities/non-generic-api.md).
{% endhint %}

It can be that simple. How does it feel to be a god?

## Changing lives

So you don't stop at changing lives, do you? Entities can be changed in a variety of ways with one or two spells.

<pre class="language-csharp"><code class="lang-csharp">// Create our beloved dwarf
var dwarf = world.Create(new Dwarf("Gimli"), new Position(0,0), new Velocity());

// Telepor the dwarf
ref var position = ref <a data-footnote-ref href="#user-content-fn-1">dwarf.Get&#x3C;Position>();</a>
position.X++;
position.Y++;

// Force him to move on its own
dwarf.Set(new Velocity(1,1));

// Give the dwarf a pickaxe and make him sad by taking it away
if(!dwarf.Has&#x3C;Pickaxe>()) <a data-footnote-ref href="#user-content-fn-2">dwarf.Add(new Pickaxe());</a>
dwarf.Remove&#x3C;Pickaxe>();
</code></pre>

{% hint style="warning" %}
The safety check using `.Has<T>()` is important. Arch does not do this itself because because our mantra is raw performance and we trust you as a developer to know when to check and when not to.
{% endhint %}

Nah great, I think you've really made the dwarf angry. Nobody takes a pickaxe away from a dwarf. Let's try to make up for it.

```csharp
// Adding components to our dwarf in batch (We could also pass them as parameters)
dwarf.Add<Pickaxe, DwarfArmor, LeatherBoots, IronHelmet>();

// Receiving components in batch and modifying them
var equipment = dwarf.Get<Pickaxe, DwarfArmor, LeatherBoots, IronHelmet>();
equipment.t0.Durability = 100f;
equipment.t1.Defence = 100f;
equipment.t3.Weight = 0.5f;

// Overwriting components since, why not? 
dwarf.Set(new DwarfArmor(110f), new LeatherBoots(Sole.GRIPPY));

// Dwarf happy now, lets ensure he does not wear any other gear by removing it
if(dwarf.Has<Bow, ElvishArmor, LongWhiteHair>()){
    dwarf.Remove<Bow, ElvishArmor, LongWhiteHair>();
}
```

{% hint style="info" %}
Up to 25 generic overloads are available, order does not matter. This often saves code and is often faster.
{% endhint %}

Looks like you've made the gnome happy again. Let's take a closer look at him then!

## Dissecting our creature

We have created life, changed it, killed it. What is still missing? Looking at it.

```csharp
var types = dwarf.GetComponentTypes();       // Getting all types, readonly
var components = dwarf.GetComponents();      // Getting all components by boxing
Console.WriteLine(dwarf);                    // Printing the entity
```

{% hint style="info" %}
The entity also has a debug view which reveals even more information.
{% endhint %}

## Pitfalls

There are **no hidden costs** in Arch, [**everything** **happens directly**](concepts.md#performance-performance-performance) without any black magic under the hood or hidden code slowing down your system. This means that everything is executed directly, without abstraction costs, the only disadvantage is that **you have to make sure** and **check** that your action makes sense at that point.

Even if it is possible to create entities during a query, change its structure or destroy it, **we recommend** [**buffering**](utilities/commandbuffer.md) **these operations and performing them after the query**. Take a look at the [examples](../examples/page-2.md)!

[^1]: `TryGet`also exists.

[^2]: \`Ensure\` also exists.
