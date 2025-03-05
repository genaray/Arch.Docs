---
description: FAQ, frequently asked questions.
---

# FAQ

<details>

<summary>Can i support this project?</summary>

Of course you can! By either becoming a [contributor](https://github.com/genaray/Arch/blob/master/CONTRIBUTING.MD) and actively participating in the ecosystem or by [supporting Arch financially](https://github.com/sponsors/genaray)! We are happy about any help!

</details>

<details>

<summary>Which projects is Arch suitable for?</summary>

Especially for **games** or **simulations**... but also data-oriented applications where it is important to have and query a lot of flexible data!

</details>

<details>

<summary>Why do other ECS-Frameworks claim to be faster?</summary>

You may have seen that Arch performs well in some benchmarks, but slower than some ECS frameworks. Does this mean that Arch is slow or poorly optimized? No, its the opposite! Every benchmark is implemented differently and sometimes uses a different (often outdated) Arch version. In addition, Arch has something that many other ECS do not have, [**Chunks**](../documentation/archetypes-and-chunks.md). [**Chunks**](../documentation/archetypes-and-chunks.md) allow you to create huge amounts of entities at runtime and even remove them later to free up memory, but this adds a bit of overhead. It's a trade-off that's worth it though!

</details>

<details>

<summary>Why doesn't Arch have XYZ?</summary>

Arch's mantra is to be [**bare minimum**](../documentation/concepts.md#archs-promise). A basic ECS that can be quickly and easily integrated anywhere with the possibility to expand it according to your own wishes. Thus we support clean code, separation of concerns and KISS as programming principles. Arch only does what it is supposed to do, to be a **high-performance ECS** that is easy to use and gives YOU the tools. Nothing more and nothing less, no hidden operations in the background. For everything else there is [**Arch.Extended**](../extensions/page-3/), a collection of tools and libraries that extend Arch.

</details>

<details>

<summary>Can i use Arch in an Engine?</summary>

You can do that too. You can use Arch anywhere you use **.Net Framework 2**, **.Net 6** to **.Net8**. This means you can use Arch in a pure C# project or in an engine like [**Unity, Godot or Stride**](https://github.com/genaray/Arch/wiki/Integration-Guides)!

</details>

<details>

<summary>Is Arch AOT compatible?</summary>

Arch also **works** in **AOT** environments. Depending on how you use it, however, you may have to [**register the components**](../documentation/utilities/component-registration.md) in advance or simply use [**Arch.Aot-SourceGenerator**](https://github.com/genaray/Arch.Extended/wiki/AOT-Source-Generator).

</details>

<details>

<summary>Can i use managed structs and classes as components with Arch?</summary>

You can do that too! In arch you can simply use classes and structs as components without having to play around with pointers or forcing only structs. This even has some advantages, it is faster to develop with it! If you want to get the **maximum performance** out of it, we still recommend using only **unmanaged structs** as components.

</details>
