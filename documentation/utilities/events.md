---
description: Nobody gets in touch with you? Here we change that!
---

# Events

It's a familiar situation: weeks have gone by again and your friends and family haven't contacted you... what a sad life. How good that you have us, because you can configure Arch so that you are informed about everything important!

**With events, you get to know about all important entity actions and can react accordingly!** This is exactly what the `EVENTS` flag is for, which you can set in Arch to enable all hooks. However, you have to modify the source code for this, it's best to take a look at the chapter on [`PURE_ECS`](../optimizations/pure\_ecs.md)!

## Become omniscient

There is not much to say. As soon as the `EVENTS` flag has been set, we can get started straight away and hook into all the important methods.

```csharp
world.SubscribeEntityCreated((Entity entity) => {});    // Listen for entity creation events
world.SubscribeEntityDestroyed((Entity entity) => {});  // Listen for entity destruction events

world.SubscribeComponentAdded((Entity entity, ref Position _) => {});    // Listen for newly added components
world.SubscribeComponentSet((Entity entity, ref Position _) => {});      // Listen for newly set components
world.SubscribeComponentRemoved((Entity entity, ref Position _) => {});  // Listen for newly removed components
```

{% hint style="info" %}
These methods are called directly after the respective operation, so there is no delay. They are almost direct method calls, i.e. very fast and efficient.
{% endhint %}

And now we know what's happening, we're finally complete control freaks!
