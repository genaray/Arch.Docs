---
description: >-
  You make your way through the thicket and suddenly there's an entity... but
  what is it?
---

# Entity

In an [entity component system (ECS)](concepts.md), an entity is simply a **unique ID** tag. It represents **an object** in the game or application, but has no logic or data itself. The data and behavior come from **components** that are attached to this entity.

It is nothing more than a simple `record struct Entity{ int ID, ... }`. Either you work directly on this entity and its methods, or you simply use `PURE_ECS`. More on this later.

## Creating life

I'm not a dark mage, but I can tell you a little about how to create life. It's actually surprisingly simple... Let's take a look!

<pre class="language-csharp"><code class="lang-csharp">var world = World.Create(); // Must exist somewhere

// Creating &#x26; Killing an entity with a string, position and velocity component
<strong>var dwarf = world.Create("Gimli", new Position(0,0), new Velocity());
</strong><strong>world.Destroy(dwarf);
</strong></code></pre>

