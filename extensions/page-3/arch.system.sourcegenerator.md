---
description: >-
  What's the point of organized queries if it's sooo much work to write them? We
  have a solution!
---

# Arch.System.SourceGenerator

I know, I know... writing queries is always sooo much work. If only you could have it all generated... wait... you can!

The `Arch.System.SourceGenerator` package provides some code generation utils. With them, queries can be written virtually by themselves which saves some boilerplate code.

**Query** methods can be generated **in** [**all classes**](#user-content-fn-1)[^1] **as long as they are partial**. However it makes most sense in the [`BaseSystem`](arch.system.md). The attributes can be used to meaningfully describe what query to generate, and the query will always call the annotated method.

## Start the engine



[^1]: Also static ones, without a `BaseSystem` inheritance.&#x20;
