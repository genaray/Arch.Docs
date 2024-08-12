---
description: >-
  Dive even deeper? You're almost at the earth's core, my dear! Let me show you
  how to insert components manually!
---

# Component Registration

You've experienced a lot along the way, but you never stop learning... right? So you've reached a point where you want to register and modify [**components**](../entity.md) yourself. Then let's take a look!

All components are recorded and stored in the `ComponentRegistry`. It primarily stores **meta data** such as the **size** and **types** of the **components**, which are then queried at runtime as a compile-time static or manually to access the underlying arrays of [`Archetypes`](../archetypes-and-chunks.md) and [`Chunks`](../archetypes-and-chunks.md) and more.

These metadata are expressed as `ComponentType`, a struct that you may have seen here and there before. It combines the component with its most important **metadata** and is used precisely for this purpose.

## Changing the (life) building blocks

There are some cases where you might prefer to register your components yourself because YOU simply know more than Arch and C# at that moment. For example, C# has **problems** with **managed structs** or even **classes** here and there. This feature is also useful for **AOT**.

```csharp
// Problematic managed struct
public record struct Inventory(List<Item> Items);

// Register, 8 = Size in bytes of the managed struct.
var componentType = new ComponentType(ComponentRegistry.Size-1, 8);
ComponentRegistry.Add(typeof(Inventory), componentType);
```

{% hint style="info" %}
This registers the **components** manually, so you can also specify a different **size** when registering.
{% endhint %}

Alternatively, you can also call up the generic variants such as...

```csharp
ComponentRegistry.Add<Inventory>();
```

```csharp
ArrayRegistry.Add<Inventory>();
```

{% hint style="info" %}
These two things do **exactly** the **same thing**, but are slightly less boilerplate code and automatically determine the **code size**.
{% endhint %}

## Acessing the (life) building blocks

You can not only register components but also receive, replace and remove.

<pre class="language-csharp"><code class="lang-csharp"><strong>// Checks whether the component is registered
</strong><strong>var hasInventoryRegistered = ComponentRegistry.Has&#x3C;Inventory>();
</strong><strong>
</strong><strong>// Try-Get
</strong><strong>if(hasInventoryRegistered)
</strong><strong>{
</strong><strong>    ComponentRegistry.TryGet&#x3C;Inventory>(out var componentType);
</strong><strong>    Console.WriteLine(componentType);
</strong><strong>}
</strong><strong>
</strong><strong>// Replaces the component with a new variant e.g. hot Hot-Loading
</strong><strong>ComponentRegistry.Replace&#x3C;Inventory, NewInventory>();
</strong><strong>
</strong><strong>// Removes the Component from registration
</strong><strong>ComponentRegistry.Remove&#x3C;Inventory>();
</strong></code></pre>

{% hint style="info" %}
Of course, there is also a non-generic variant for all generic methods!
{% endhint %}

## Compile-Time static&#x20;

So now you've registered all these wonderful components... but how does Arch get to it under the hood? And how can you do it yourself? Of course this works by accessing the `ComponentRegistry`, but this is always associated with a lookup. There has to be an more efficient way!

```csharp
var componentType = Component<Inventory>.ComponentType;
```

Using the `Component<T0...T25>`is one way. This is a compile-time static class that provides all meta-data for the passed `Component` directly, **without any lookups**! And the best, it comes with up to **25 generic overloads**!

```csharp
var signature = Component<Dwarf, Axe, Inventory>.Signature;
```

{% hint style="info" %}
When passing multiple generics, it returns a `Signature`. A struct which contains multiple `ComponentTypes` from the different passed types.&#x20;
{% endhint %}

Cool, right? Now you got the tools to build almost everything you want on your own! E.g. custom librarys and extensions!
