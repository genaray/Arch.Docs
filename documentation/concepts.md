---
description: >-
  ECS, a data oriented architecture bringing flexibility, reusable components
  and performance to you.
---

# Concepts

There you are again! I've missed you already, we haven't seen each other for a long time. I beg your pardon? You're asking what's behind Arch and an ECS? Then you'd better buckle up.

Arch is an [Entity-Component System](https://www.wikiwand.com/en/Entity_component_system) (ECS) which is an architectural pattern that favors composition over inheritance. [**Entities**](entity.md) are small, unique identifiers, and [**Components**](entity.md#changing-lives) are data attached to them. The term [**Systems**](query.md) often refers to the logic that operates on this data.

## ECS in a nutshell

<details>

<summary>Entities exist in a World</summary>

The world contains all entities that exist in its context. No entity can live outside of this world. Entities can be created individually, in bulk and with predefined data.

</details>

<details>

<summary>Components can be added to / removed from Entities</summary>

The structure of entities can be changed at any time. Components can be classes, but also structs or primitives.

</details>

<details>

<summary>Queries filter entities by match</summary>

Entities are filtered and matched by Component types. You define which entities with which components you want and Arch searches for them for you, quickly, easily and efficiently.

</details>

<details>

<summary>The code processes the component data in tight, optimized loops.</summary>

You specify a delegate/lambda to tell Arch how to process the data. There are of course also ways to iterate manually over the entities and components.

</details>

<details>

<summary>Component data is always kept contiguous in Memory.</summary>

Structurally identical entities are packed into archetypes and chunks to improve cache locality. Each chunk is a memory block of exactly 64KB that is loaded quickly and efficiently into the L1 cache to improve performance.

</details>

## Archs promise

<details>

<summary>Bare minimum (yet expandable and rich)</summary>

Arch is small and simple. No unnecessary features, no mechanisms that run in the background and consume unnecessary performance even though you don't need them. At the same time, Arch is easily extensible and has a rich ecosystem and many optional features.

</details>

<details>

<summary>Performance, Performance, Performance...</summary>

Arch is designed for maximum speed and efficiency. Each method is optimized to perform its task directly, without additional background checks or validation. This ensures impressive performance for demanding applications. Arch puts you in the driver's seat and trusts you as a developer to make the most of it! For the faint of heart, you can of course include the source code with debug flags.

</details>

<details>

<summary>Community driven</summary>

Arch is from the community for the community. You can participate, create forks, contribute and improve. In fact, this is all encouraged!

</details>

## Additional features

<details>

<summary><a href="concepts.md#archetypes-and-chunks">Archetypes &#x26; Chunks</a></summary>

Arch not only has archetypes but also chunks. This sets it apart from many other ECSs. Each archetype has several chunks. Chunks themselves are 64KB memory blocks and contain the entities and their components directly. The big advantage of this is that it is more efficient to iterate and allocate. Fast, efficient and memory friendly.

</details>

<details>

<summary><a href="optimizations/pure_ecs.md">Small entities and `PURE_ECS`</a></summary>

Arch's entities are as small as possible, no unnecessary data is stored. If you want to make your entities even smaller, Arch supports the concept of “Pure ECS”. This makes entities even smaller and more memory-friendly.

</details>

<details>

<summary><a href="optimizations/query-techniques.md">Versatile and fast queries</a></summary>

You don't feel like writing delegates to edit entities? Don't worry, arch has you covered! We support a variety of different query types, from delegates to interfaces to manual enumeration and even source generation!

</details>

<details>

<summary><a href="concepts.md#bulk-and-batch">Bulk &#x26; Batch</a></summary>

It is also possible to create, change and delete entities in bulk. This is even more efficient than doing this for each entity individually, Arch is incredibly efficient.

</details>

<details>

<summary><a href="optimizations/multithreading.md">Multithreading &#x26; Jobs</a></summary>

For even more efficiency and large amounts of data and entities, there is multithreading and jobs. The best thing about it, without creating garbage. Arch has its own JobScheduler that was written just for this purpose. This allows you to bring your huge worlds to life even better!

</details>

<details>

<summary><a href="concepts.md#commandbuffer">CommandBuffer</a></summary>

You don't want to make changes to entities immediately but only later? No problem! Arch has even taken this into consideration: with the CommandBuffer you can postpone these changes and execute them at a later time.

</details>

<details>

<summary><a href="concepts.md#events">Events</a></summary>

Entity events are one of the things that are not included by default in the nugget, but you can easily enable them through the source code using a flag! You only pay for what you use!

</details>

<details>

<summary><a href="utilities/non-generic-api.md">Generic &#x26; Non-Generic API</a></summary>

Of course, you can also use the API with simple types. Nobody is forcing you to use generics. This is often very helpful, especially when it comes to persistence and reflection.

</details>

<details>

<summary>And much much more...</summary>

There's so much more to discover, from extensions like source generators and tools, to integration guides for your favorite engines and more. Dive in and find out for yourself!

</details>

Let's start! First you should get familiar with the [`World`](world.md)`!`
