---
description: Your journey begins right here, we are excited to see what you create!
cover: .gitbook/assets/FFjUBkDk7GvimorPpENDf.png
coverY: 65.64701175293823
layout:
  cover:
    visible: true
    size: full
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

# 🌄 The start of your journey

### The dream <a href="#the-dream" id="the-dream"></a>

Hey, you. You are finally awake. You were trying to cross the border, right? I bet you're wondering where you've ended up and what Arch actually is, aren't you? I'm glad you asked!&#x20;

Well, there's probably a lot to talk about, but let's start with the essentials.Arch is a high-performance C# based Archetype & Chunks [Entity Component System](https://www.wikiwand.com/en/Entity\_component\_system) (ECS) for game development and data-oriented programming. To summarize:

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

Supports .NetStandard 2.1, .Net Core 6 and 7, and therefore you may use it with Unity or Godot!

</details>

### Preparing for an adventure <a href="#preparing-for-an-adventure" id="preparing-for-an-adventure"></a>

Put on your boots and give it a try, it's easier than you thought...

```sh
dotnet add PROJECT package Arch --version 1.2.8.1-alpha
```

Don't miss your luggage and briefly import Arch...

```csharp
using Arch;
```

And your journey can begin! It wasn't difficult, was it? Let's take a look at this example together, it should open your tired eyes and show you where the journey is heading.

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

### Setting off on an Odyssey <a href="#setting-off-on-an-odyssey" id="setting-off-on-an-odyssey"></a>

You now know what Arch is and have added it to the project... Now you're standing there with your luggage and boots and don't know where to go? You're growing up so fast, let me help you.

The world is so big and multifaceted. Next, you could plunge into the lake of[ documentation](broken-reference), climb the great mountain of [examples ](broken-reference)or venture into the deep mines of the[ Extensions](broken-reference). It's hard for me to let you go, but I'm so excited to see where it will take you.

## The world of open source...

But before you leave, take a moment to look at this beautiful world... Do you notice anything? It is completely **open source**! You can **contribute** and **change** every aspect! So if you like it, [**leave a star** ⭐](https://github.com/genaray/Arch) and [**buy us a coffee** ☕](https://github.com/sponsors/genaray)!
