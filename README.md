---
description: Your journey begins right here, we are excited to see what you create!
---

# The start of your journey

### The dream <a href="#the-dream" id="the-dream"></a>

Hey, you. You are finally awake. You were trying to cross the border, right? I bet you're wondering where you've ended up and what Arch actually is, aren't you? I'm glad you asked!&#x20;

Well, there's probably a lot to talk about, but let's start with the essentials.Arch is a high-performance C# based Archetype & Chunks [Entity Component System](https://www.wikiwand.com/en/Entity\_component\_system) (ECS) for game development and data-oriented programming. To summarize:

* 🏎️ _**FAST**_ > Best cache efficiency, iteration, and allocation speed. Plays in the same league as C++/Rust ECS Libs!
* 🚀 _**FASTER**_ > Arch is on average quite faster than other ECS implemented in C#. Check out this [Benchmark](https://github.com/Doraku/Ecs.CSharp.Benchmark)!
* 🤏 _**BARE MINIMUM**_ > Not bloated, it's small and only provides the essentials for you!
* ☕️ _**SIMPLE**_ > Promotes a clean, minimal, and self-explanatory API that is simple by design. Check out the [Wiki](https://github.com/genaray/Arch/wiki)!
* 💪 _**MAINTAINED**_ > It's actively being worked on, maintained, and comes along several [Extensions](https://github.com/genaray/Arch.Extended)!
* 🚢 _**SUPPORT**_ > Supports .NetStandard 2.1, .Net Core 6 and 7, and therefore you may use it with Unity or Godot!

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

// Components ( ignore the formatting, this saves space )
public struct Position{ float X, Y };
public struct Velocity{ float Dx, Dy };

public sealed class Game 
{
    public static void Main(string[] args) 
    {     
        // Create a world and entities with position and velocity.
        var world = World.Create();
        for (var index = 0; index < 1000; index++) 
            world.Create(new Position{ X = 0, Y = 0}, new Velocity{ Dx = 1, Dy = 1});
        
        // Enumerate entities with Position AND Velocity to modify them
        var query = new QueryDescription().WithAll<Position,Velocity>();
        world.Query(in query, (Entity entity, ref Position pos, ref Velocity vel) => {
            pos.X += vel.Dx;
            pos.Y += vel.Dy;
            Console.WriteLine($"Moved: {entity.Id}"); 
        }); 
    }
}
```

### Setting off on an Odyssey <a href="#setting-off-on-an-odyssey" id="setting-off-on-an-odyssey"></a>

You now know what Arch is and have added it to the project... Now you're standing there with your luggage and boots and don't know where to go? You're growing up so fast, let me help you.

The world is so big and multifaceted. Next, you could plunge into the lake of documentation, climb the great mountain of examples or venture into the deep mines of the Extensions. It's hard for me to let you go, but I'm so excited to see where it will take you.
