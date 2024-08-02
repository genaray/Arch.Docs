---
description: A new order is needed to command your entities even more efficiently.
---

# Query-Techniques

Arch has a number of different ways to search and iterate over entities. There are currently exactly 4 ways to issue commands to your entities in queries.

While this may seem a bit complex at first, each of these ways has advantages and disadvantages and is perfect for specific situations. Let us start...

{% hint style="info" %}
All the queries listed below have been kept simple; each variant naturally also has the option of accessing the [`entity`](../entity.md) itself or other functionalities.
{% endhint %}

## Query

Firstly, there is the type of query with which all examples were listed. Small and legible, you hardly have to worry.

<pre class="language-csharp"><code class="lang-csharp">World.Query(in desc, <a data-footnote-ref href="#user-content-fn-1">(ref Position pos, ref Velocity vel)</a> => {
   Move(ref pos, ref vel);
});
</code></pre>

{% hint style="success" %}
* Less code
* Can use methods/delegates
* Great for prototyping
{% endhint %}

{% hint style="danger" %}
* Uses delegates, which is slow that the code can not be inlined.
* All components can only be passed with ref, no security.
* Rather slow compared to other techniques, still fast though.
* Enclosure copies probably.
{% endhint %}

## Inline-Query

As an alternative, we have the slightly faster version, which is also wonderfully reusable by distributing its tasks in structs.

<pre class="language-csharp"><code class="lang-csharp">public struct VelocityUpdate : <a data-footnote-ref href="#user-content-fn-2">IForEach</a>&#x3C;Position, Velocity> {

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void Update(ref Position pos, ref Velocity vel) { 
        Move(ref pos, ref vel);
    }
}

world.<a data-footnote-ref href="#user-content-fn-3">InlineQuery</a>&#x3C;VelocityUpdate, Position, Velocity>(<a data-footnote-ref href="#user-content-fn-4">in queryDescription</a>);  
</code></pre>

{% hint style="success" %}
* Damn fast
* Uses struct and interface, can be inlined
* Allows an order and modularization of the code
* An instance of the structs can be passed through to process data
{% endhint %}

{% hint style="danger" %}
* Requires more boilerplate code
* All components can only be passed with ref, no security.
{% endhint %}

## Custom enumeration

A little more manual labour, but an incredible amount of flexibility. This gives you the opportunity to design everything according to your wishes!

<pre class="language-csharp"><code class="lang-csharp">var query = world.Query(in desc);
foreach(ref var chunk in query.GetChunkIterator())
{
   var references = <a data-footnote-ref href="#user-content-fn-5">chunk.GetFirst&#x3C;Position, Velocity>;</a>  
   <a data-footnote-ref href="#user-content-fn-6">foreach(var entity in chunk)   </a>                       
   {
       <a data-footnote-ref href="#user-content-fn-7">ref var pos = ref Unsafe.Add(ref references.t0, entity);</a>
       ref var vel = ref Unsafe.Add(ref references.t1, entity);
       Move(ref pos, ref vel);
   }
}
</code></pre>

{% hint style="success" %}
* The fastest query variant
* Is also inlined
* Allows custom query logic
* SIMD
* Own multithreading
* Great for hotpaths
{% endhint %}

{% hint style="danger" %}
* Requires the most boilerplate code
* Bad for prototypes or fast coding
{% endhint %}

## Source-Generation

The last variant is the source-generated queries. These are provided by [Arch's rich ecosystem](https://github.com/genaray/Arch.Extended) and can be integrated separately. They generate the code for you as if you had written it manually. Fast, efficient and simple. What more could you want?

<pre class="language-csharp"><code class="lang-csharp">[Query] 
// [All&#x3C;...>, Any&#x3C;...>, None&#x3C;...>] to filter
// [Parallel] for multithreading
[MethodImpl(MethodImplOptions.AggressiveInlining)]
public void MoveEntity(<a data-footnote-ref href="#user-content-fn-8">[Data] ref float time</a>, ref Position pos, <a data-footnote-ref href="#user-content-fn-9">ref</a> Velocity vel)  
{
    Move(ref pos, ref vel, ref time);
}
</code></pre>

{% hint style="success" %}
* Same as the [custom query](query-techniques.md#custom-enumeration)
* Less code
* Declarative syntax
* Allows passing on data into the query with ease
* You can use `in`, `ref` and `out` for components
{% endhint %}

{% hint style="danger" %}
* Requires [Arch.Extended](https://github.com/genaray/Arch.Extended)
* Code generator, probably not available for all platforms
{% endhint %}

[^1]: Or as an alternative with an entity that is passed through.

[^2]: `IForEachEntity` if you wanna acess the [`Entity`](../entity.md) aswell.&#x20;

[^3]: `InlineEntityQuery` if an `IForEachEntity` is used.

[^4]: A struct instance can also be passed through if data is to be processed in the struct.

[^5]: Returns the beginnings of the arrays to iterate over them manually. There are some other methods that do this as well, but this one is recommended.

[^6]: Enumerates over each entity in this chunk.

[^7]: Receives a reference to the component of the entity.&#x20;

[^8]: Optional, can also be left out. You can specify parameters with Data to pass them through.

[^9]: `ref`, `in`, `out` possible to define data-acess.&#x20;
