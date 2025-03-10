---
description: Arch.AOT.SourceGenerator, making arch fully AOT compatible.
---

# Arch.AOT.SourceGenerator

Arch itself in its current state is **mostly** native **AOT** **compatible**. However, it still requires some boilerplate-code, especially the registration of components. That's where this extension comes into play.

## Example

You could register all your components by yourself. But that's a lot of work. AOT.SourceGenerator got you, just mark your components with a `[Component]` attribute like this:

```csharp
[Component]
public struct Velocity{ ... }

[Component]
public struct Transform{ ... }
```

And it will generate a registration-module like this:

<pre class="language-csharp"><code class="lang-csharp">namespace Arch.AOT.SourceGenerator
{
   internal static class GeneratedComponentRegistry
   {
      [ModuleInitializer]
      public static void Initialize()
      {
	  <a data-footnote-ref href="#user-content-fn-1">ComponentRegistry.Add(new ComponentType(ComponentRegistry.Size + 1, typeof(Velocity), Unsafe.SizeOf&#x3C;Velocity>(), false);</a>
          ArrayRegistry.Add&#x3C;Velocity>();
          
          ComponentRegistry.Add(new ComponentType(ComponentRegistry.Size + 1, typeof(Transform), Unsafe.SizeOf&#x3C;Transform>(), false);
          <a data-footnote-ref href="#user-content-fn-2">ArrayRegistry.Add&#x3C;Transform>();</a>
      }
   }
}
</code></pre>

{% hint style="info" %}
If you prefer to do the whole thing manually, have a look [here](../../documentation/utilities/component-registration.md)...
{% endhint %}

Notice the `[ModuleInitializer]`? The generated class will automatically be called upon application start to register the components.

[^1]: Registers the component in the `ComponentRegistry.`&#x20;

[^2]: Registers the component in the `ArrayRegistry`, this is important for providing native AOT arrays of the component type.&#x20;
