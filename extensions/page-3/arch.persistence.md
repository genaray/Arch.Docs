---
description: Arch.Persistence, a tiny persistence framework that supports JSON & Binary.
---

# Arch.Persistence

With this package you can easily convert your worlds and entities to **JSON** and **binary** and back. This is useful for **save files**, **databases** or **networking**, for example.&#x20;

Both serializers support (de)serialization with bytes and streams. The binary one also supports the newly introduced `IBufferWriter<T>` for streaming purposes.

{% hint style="warning" %}
**Limitations**:

* Single Entity serialization does not support cyclic references and requires custom handling.
* World serialization can handle cyclic entity references.
* Can not handle cross-world references since those are literally unresolvable.
* The order in which components are [registered](../../documentation/utilities/component-registration.md) must be the same for deserialization as for serialization.
{% endhint %}

## JSON

JSON is the ultimate data exchange format. If you don't know it, you should really catch up. The usage is very very simple and flexible. Take a look at the [git-example project](https://github.com/genaray/Arch.Extended/blob/master/Arch.Extended.Sample/Game.cs#L74) or check out the example!

```csharp
// Initialize the Serializer, and inject custom ones to handle classes or structs in a specific way.
var serializer = new ArchJsonSerializer(new SpriteSerializer{GraphicsDevice = GraphicsDevice}, ...);

var worldJson = serializer.ToJson(_world);   // Serialize your world to a json-string
_world = serializer.FromJson(worldJson);     // Deserialize it from a json-string.

var entityJson = serializer.ToJson(_world, someEntity);  // Serialize single Entity to json.
serializer.FromJson(_world, entityJson);         // Entity is being deserialized to the given world. 

// Example Custom Serializer for Sprites
public class SpriteSerializer : IJsonFormatter<Sprite>
{
    public void Serialize(ref JsonWriter writer, Sprite value, IJsonFormatterResolver formatterResolver)
    {
        // Your logic
    }

    public Sprite Deserialize(ref JsonReader reader, IJsonFormatterResolver formatterResolver)
    {
        // Your logic
    }
}
```

## Binary

Binary is fast and efficient. However, not human readable. For certain purposes, it is nevertheless perfect.

```csharp
// Initialize the Serializer, and inject custom ones to handle classes or structs in a specific way.
var serializer = new ArchBinarySerializer(new SpriteSerializer{GraphicsDevice = GraphicsDevice}, ...);

var worldBytes = serializer.Serialize(_world);   // Serialize your world to a byte[]
_world = serializer.Deserialize(worldBytes );    // Deserialize it from a byte[].

var entityBytes = serializer.Serialize(_world, someEntity);  // Serialize single Entity to byte[].
serializer.Deserialize(_world, entityBytes);                 // Entity is being deserialized to the given world. 

// Example Custom Serializer for Sprites
public class SpriteSerializer : IMessagePackFormatter<Sprite>
{
    public void Serialize(ref MessagePackWriter writer, Sprite value, MessagePackSerializerOptions options)
    {
        // Your logic
    }

    public Sprite Deserialize(ref MessagePackReader reader, MessagePackSerializerOptions options)
    {
        // Your logic
    }
}
```

Since the package uses Messagepack under the hood, you can easily extend it at any time. For an insight into how to create custom serializers, check out the [project](https://github.com/MessagePack-CSharp/MessagePack-CSharp)!
