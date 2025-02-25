---
description: Arch, a high-performance and bare minimum C# ECS.
cover: .gitbook/assets/arch-banner-2.png
coverY: 547.5749999999999
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🌄 Why Arch?

Arch is a high-performance C# based Archetype & Chunks [Entity Component System](https://www.wikiwand.com/en/Entity_component_system) (ECS) for game development and data-oriented programming. To summarize:

<details>

<summary>🏎️ FAST </summary>

Best cache efficiency, iteration, and allocation speed. Plays in the same league as C++/Rust ECS Libs!

</details>

<details>

<summary>🚀 <em><strong>FASTER</strong></em></summary>

Arch is on average quite faster than other ECS implemented in C#. Check out this [Benchmark](https://github.com/Doraku/Ecs.CSharp.Benchmark)!

</details>

<details>

<summary>🤏 <em><strong>BARE MINIMUM</strong></em></summary>

Not bloated, it's small and only provides the essentials for you!

</details>

<details>

<summary>☕️ <em><strong>SIMPLE</strong></em> </summary>

Promotes a clean, minimal, and self-explanatory API that is simple by design. Check out the [Wiki](https://github.com/genaray/Arch/wiki)!

</details>

<details>

<summary>💪 <em><strong>MAINTAINED</strong></em> </summary>

It's actively being worked on, maintained, and comes along several [Extensions](https://github.com/genaray/Arch.Extended)!

</details>

<details>

<summary>🚢 <em><strong>SUPPORT</strong></em> </summary>

Supports .NetStandard 2.1, .Net Core 8, and therefore you may use it with Unity, Godot or any other C#-Project!

</details>

## Example <a href="#preparing-for-an-adventure" id="preparing-for-an-adventure"></a>

Put on your boots and give it a try, it's easier than you thought...

```sh
dotnet add PROJECT package Arch --version 1.3.3-alpha
```

Don't miss your luggage and briefly import Arch...

```csharp
using Arch;
```

And your journey can begin! It wasn't difficult, was it? Let's take a look at this example together, it should open your eyes and show you where the journey is heading.

```csharp
using Arch;

// Components
public record struct Position(float X, float Y);
public record struct Velocity(float Dx, float Dy);

// Create a world and an entity with position and velocity.
using var world = World.Create();
var adventurer = world.Create(new Position(0,0), new Velocity(1,1));

// Enumerate all entities with Position & Velocity to move them
var query = new QueryDescription().WithAll<Position,Velocity>();
world.Query(in query, (Entity entity, ref Position pos, ref Velocity vel) => {
    pos.X += vel.Dx;
    pos.Y += vel.Dy;
    Console.WriteLine($"Moved adventurer: {entity.Id}"); 
}); 
```

{% hint style="info" %}
This example is just a foretaste, more syntax and API await you on your adventure! Even non-generic ones and some without lambdas!
{% endhint %}

## Next steps <a href="#setting-off-on-an-odyssey" id="setting-off-on-an-odyssey"></a>

Where to next? Arch is packed with features. Look at the[ documentation](broken-reference), play around with the [examples ](broken-reference)or make yourself familiar with the[ Extensions](broken-reference). It's hard for me to let you go, but I'm so excited to see where it will take you.

## Socials

Get involved!

* [Github](https://github.com/genaray/Arch)
* [Discord](https://discord.gg/htc8tX3NxZ)

## The world of open source...

But before you leave, take a moment to value this project. Do you notice anything? It is completely **open source**! You can **contribute** and **change** every aspect! So if you like it, [**leave a star** ⭐](https://github.com/genaray/Arch) and [**buy us a coffee** ☕](https://github.com/sponsors/genaray) to support further development!
