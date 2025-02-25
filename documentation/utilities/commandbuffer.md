---
description: CommandBuffer, to record operations and play them back at any time.
---

# CommandBuffer

It's often like this... we have a lot planned, but we can't do it all at once, **we have to work through it piece by piece**. The same applies to your world and entities, of course... and we have something for that here! The **CommandBuffer** is exactly for that. You can give it your **commands**, **it saves them** and **executes them at a later time**. Practical, especially for **multithreading** and if you want to postpone changes until later.

<pre class="language-csharp"><code class="lang-csharp">// Our dwarf
var dwarf = world.Create(new Dwarf(), new Position(), new Velocity());

// Buffering actions on entities to play them back later
var commandBuffer = new CommandBuffer();
<a data-footnote-ref href="#user-content-fn-1">commandBuffer.Add(dwarf, new Pickaxe();</a>
commandBuffer.Set(dwarf, new Position(10,10);

// Buffering a whole entity
<a data-footnote-ref href="#user-content-fn-2">var nonExistingDwarf = commandBuffer.Create(new Signature(typeof(Dwarf), typeof(Position), typeof(Velocity));</a>
<a data-footnote-ref href="#user-content-fn-3">commandBuffer.PlayBack(world, disposal = true);</a>
</code></pre>

{% hint style="info" %}
Actions on entities can therefore be postponed and executed at a later time. This is sometimes more effective than executing them directly, all actions are thread-safe from the`CommandBuffer` and can also be executed in `ParallelQuery`s.
{% endhint %}

And now we can record changes and play them back at any time, that's cool, isn't it?

[^1]: Do not add the components immediately, but at a later time.

[^2]: Not only can changes be postponed, but entire [`entities`](../entity.md) can also be created and edited at a later date.

[^3]: Transfers the changes into the [**world**](../world.md). Executes the changes. If \`disposal\` is false, we can playback the same changes multiple times without rerecording them.
