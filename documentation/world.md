---
description: Now you have taken courage and set off into the big wide world.
---

# World

The world is a great place, isn't it? Huge, beautiful and full of life. There is so much to discover, let's create one of our own.

The **world stores all its entities**, it contains methods to create, destroy and query them and handles all the internal mechanics. Therefore it is the most important class, you will use the world heavily. Time to play God.

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

The world has the rudimentary methods on which everything in arch is based. If you only want to work with them, this is possible and offers one less level of abstraction. We'll go into more detail later.                       &#x20;

But now enough talk, go on. Something is lurking behind the next bush.
