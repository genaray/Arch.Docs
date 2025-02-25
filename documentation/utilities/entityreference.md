---
description: EntityReference, keep entities apart.
---

# EntityReference

You already know what an [`Entity`](../entity.md)  is. However, one thing has not yet been mentioned: the **ID** of an entity **can** **reappear** from time to time. This is called **recycling**, if an [`Entity`](../entity.md) with the ID of ‘25’ is destroyed, one of the next entities will receive its ID. An ID will never appear more than once at the same time, but it can appear more often over time.

So it is possible that you have saved an [`Entity`](../entity.md) struct somewhere that **originally** refers to a **dwarf**... This is destroyed, you don't realise it and the next time you access it, it's **suddenly** an **elf**.

To prevent this and work memory-efficiently at the same time, there is `EntityReference` for this purpose.

<pre class="language-csharp"><code class="lang-csharp">var entity = world.Create&#x3C;Dwarf, Position, Velocity>();
var reference = <a data-footnote-ref href="#user-content-fn-1">entity.Reference();</a>

// Checks if the entity is the same as before by comparing the version and its id
if(reference.IsAlive()) {
    var referencedEntity = ((Entity)reference);
    Console.WriteLine(referencedEntity);
}     
</code></pre>

{% hint style="info" %}
If the original [`Entity`](../entity.md) to which the `EntityReference` points is destroyed, `EntityReference.IsAlive()`will return false. Even if the ID of the [`Entity`](../entity.md) has been recycled. **It therefore ensures that we only work with the referenced** [**`Entity`**](../entity.md)**.**
{% endhint %}

And now you can safely and easily handle references to other entities, great, right?

[^1]: Creates a `EntityReference` to the [`Entity`](../entity.md). The [`World`](../world.md) also features such a method if [`PURE_ECS`](../optimizations/pure_ecs.md) is used.&#x20;
