---
description: Unity and how to use Arch with it.
---

# Unity

How do I actually use Arch in Unity? There are several ways, let's take a look at them because all of them have advantages and disadvantages.

## Dlls

The easiest way is to integrate Arch through its dlls. When building each version, dlls for the different versions are created and uploaded to [Arch CI/CD Pipeline](https://github.com/genaray/Arch/actions). You can simply put them in the assets/plugins folder and you can use arch everywhere. The only disadvantage is that you need not only Arch dlls but also the dlls of Arch's dependencies. This includes `Arch.LowLevel`, `Collections.Pooled` and possibly `Arch.System` and co depending on what you want to use additionally. An example project can be found [here](https://github.com/decentraland/unity-explorer/tree/dev/Explorer/Assets/Plugins/Arch).

## NuGetForUnity

Another option is to use [Unity Nuget.](https://github.com/GlitchEnzo/NuGetForUnity) This allows you to integrate nuggets such as the one from Arch relatively easily and makes things a lot easier for you.

## Sources

As a last resort, it is also possible to simply copy Arch's source code into unity. But since Arch uses a newer C# version, it is possible that some methods are not available and you have to replace them manually with Unity-compatible methods.
