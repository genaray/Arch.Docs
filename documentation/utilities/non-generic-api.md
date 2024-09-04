---
description: >-
  Immerse yourself in the dark magic of the non-generic API. But watch out, it's
  going to be a wild ride!
---

# Non-generic API

Sometimes we **don't know** the **types** and **generics** at runtime... For example, if we do a lot of dynamic programming such as saving/loading or similar cases. Sometimes we simply cannot express everything using generics, but are forced to fall back on types. And that's where this chapter comes in!

So far, you have only learned about the generic API, which is primarily used in all examples. However, for EVERY single generic method there is also a counterpart that is not generic. This variant uses `ComponentType` and `Signature`.

## The backbone

Generic values are always known at compile time. To make these available at runtime, there is first of all `ComponentType`. This abstracts a component, i.e. expresses the **component** and its **metadata**.

<pre class="language-csharp"><code class="lang-csharp"><a data-footnote-ref href="#user-content-fn-1">var componentType = Component&#x3C;Dwarf>.ComponentType;  </a>   
Console.WriteLine(componentType);  // See what meta-data exists
</code></pre>

{% hint style="info" %}
This compile-time-static method registers the component and returns its meta data. This happens under the hood.
{% endhint %}

```csharp
var componentType = ComponentRegistry.GetComponentType(typeof(Dwarf));
```

{% hint style="info" %}
However, it is slower than the generic variant. An alternative to this is the `ArrayRegistry`, which is also used to access meta data, it uses the `ComponentRegistry` under the hood.
{% endhint %}

And we have already received the `ComponentType` without knowing it at runtime. Practical, isn't it? Well, we're almost there, but there's one more important thing.

## Signature

You now know what a `ComponentType` is. What is a `Signature`? In the end, a `Signature` is just a collection of `ComponentTypes`. It is used in different areas, for example to specify the structure of an [`Entity`](../entity.md) without generics.

```csharp
var signature = new Signature(typeof(Dwarf), typeof(Position), typeof(Velocity));
var dwarf = world.Create(signature);
```

{% hint style="info" %}
The generic alternative to this is `Component<Dwarf, Position, Velocity>.Signature`, which is used in many places under the hood.
{% endhint %}

A `Signature` is therefore used in many different places. Among other things, it is also used to define queries.

```csharp
var queryDesc = new QueryDescription(
    all: new Signature(typeof(Dwarf), typeof(Velocity), typeof(Position));
    any: Signature.Null;
    none: Signature.Null;
);
world.Query(in queryDesc, entity => {});
```

It's as easy as this. Now you should know roughly what a `Signature` is good for, shouldn't you?

## Changing lives

You should have the basics down now, right? Otherwise there are many other cases where the non-generic API is used.

```csharp
// Create our beloved dwarf
var signature = new Signature(typeof(Dwarf), typeof(Position), typeof(Velocity));
var dwarf = world.Create(signature);

// Telepor the dwarf
var position = dwarf.Get(typeof(Position));

position.X++;
position.Y++;

// Force him to move on its own
dwarf.Set(typeof(Position), position);

// Give the dwarf a pickaxe and make him sad by taking it away
if(!dwarf.Has(typeof(Pickaxe))) dwarf.Add(typeof(Pickaxe), new Pickaxe());
dwarf.Remove(typeof(Pickaxe));
```

What more could you want? Now you can manage and command your entities generically and without a generic API, a bright future lies ahead of you!&#x20;

[^1]: It is also possible to obtain the `ComponentType` from several `Types` at the same time: `Component<Dwarf, Pickaxe>.ComponentType;`
