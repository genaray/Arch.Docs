---
description: >-
  Multithreading, the simultaneous iteration and modification of entities for
  extra speed.
---

# Multithreading

Imagine that you have millions of entities and you want to move them all. It gets a bit difficult on a CPU core. But we have a solution for that too!

This is not a problem, because Arch comes with its own [**`JobScheduler`**](https://github.com/genaray/ZeroAllocJobScheduler). This does not generate any garbage and is incredibly fast, even has dependencies and co. What more could you want? Let's be honest!

## Setup

Not much is needed to set up multithreading. You set it up, you know it and that's it!

<pre class="language-csharp"><code class="lang-csharp">// Create Scheduler and assign it to world
var jobScheduler = new(
  new JobScheduler.Config
  {
    ThreadPrefixName = "Arch.Samples",
    <a data-footnote-ref href="#user-content-fn-1">ThreadCount = 0</a>,                         
    MaxExpectedConcurrentJobs = 64,
    StrictAllocationMode = false,
  }
);
<a data-footnote-ref href="#user-content-fn-2">world.SharedJobScheduler = jobScheduler;</a>

// To dispose the JobScheduler at the end of the lifecycle.
jobScheduler.Dispose();
</code></pre>

{% hint style="info" %}
So the world uses the `JobScheduler` and you don't need direct contact to it, but if you want to, [**you can also access it directly**](https://github.com/genaray/ZeroAllocJobScheduler)!
{% endhint %}

## Example

Wonderful, now we have set up the `JobScheduler`, all we need to do is let it work to utilise the full capacity of our power!

<pre class="language-csharp"><code class="lang-csharp">var queryDesc = new QueryDescription().WithAll&#x3C;Dwarf, Position, Velocity>();
<a data-footnote-ref href="#user-content-fn-3">world.ParallelQuery</a>(in queryDesc, <a data-footnote-ref href="#user-content-fn-4">(ref Position pos, ref Velocity vel)</a>{
    Move(ref pos, ref vel);
});
</code></pre>

{% hint style="info" %}
This runs a query that processes the entities simultaneously on several cores of your CPU. Arch does everything for you by itself, all you have to do is set it up and change the name of the [`Query`](../query.md) here and there... cool right? Just be careful, you have to synchronise access to other objects yourself!
{% endhint %}

And now we have EVEN more power to march our armies, marvellous, isn't it? Your computer will probably take off, but it's worth it.

Of course, every form of [`Query`](../query.md) is supported again. Even [**inline queries**](query-techniques.md#inline-query)**.**

<pre class="language-csharp"><code class="lang-csharp">public struct VelocityUpdate : IForEach&#x3C;Position, Velocity> {
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void Update(ref Position pos, ref Velocity vel) { 
        Move(ref pos, ref vel);
    }
}

world.InlineParallelQuery&#x3C;VelocityUpdate, Position, Velocity>(<a data-footnote-ref href="#user-content-fn-5">in queryDescription</a>); 
</code></pre>

{% hint style="warning" %}
Each of these calls, whether `world.ParallelQuery` or `world.InlineParallelQuery`, will **block** the **main thread** **until** the [`Query`](../query.md) has been **processed**. The processing itself still takes place in parallel, but the main thread waits before continuing.
{% endhint %}

## Lowlevel

If this is all too high level for you, there is also the option of tackling the problem further down. Not below the belt, of course!

<pre class="language-csharp"><code class="lang-csharp">public struct VelocityUpdate : IChunkJob{

   public void Execute(ref Chunk chunk) {
      
      ref var positions = ref chunk.GetFirst&#x3C;Position>();
      ref var velocities = ref velocity.GetFirst&#x3C;Velocity>();

      foreach(var entity in chunk){

         ref var position = ref Unsafe.Add(ref positions, entity);
         ref var velocity = ref Unsafe.Add(ref velocities, entity);
         Move(ref position, velocity);
      }
   }
}

world.InlineParallelChunkQuery(in query, <a data-footnote-ref href="#user-content-fn-6">new VelocityUpdate()</a>);
</code></pre>

{% hint style="info" %}
This is what it looks like under the bonnet. This is what happens when you call the other two queries, so you can take over the whole thing yourself and add your own logic!
{% endhint %}

And you'll have even more power for everything you set out to do... honestly, what do you want with so much power anyway?

[^1]: 0 means determine at runtime

[^2]: Worlds can share \`JobScheduler\`, so in theory only one is required.

[^3]: Sounds familiar, doesn't it? Looks like a normal [`Query`](../query.md), but runs in the `JobScheduler` and takes care of everything on its own!

[^4]: Of course, you can also enter the [`Entity`](../entity.md) here, as with a normal [`Query`](../query.md). But you don't have to.

[^5]: A struct instance can also be passed through if data is to be processed in the struct.

[^6]: Can also be omitted, in which case a default instance is used.
