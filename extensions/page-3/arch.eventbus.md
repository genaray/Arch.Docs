---
description: Arch.EventBus, a high-performance way of sending notifications or events.
---

# Arch.EventBus

To decouple code and outsource logic using events, `Arch.Extended` has the `EventBus`. The special feature here, it is completely source generated and therefore as fast as native method calls. And it can even be **inlined** and **supports static classes and instances**.

## Example

```csharp
public partial class MyInstanceReceiver {

   public MysInstanceReceiver(){ Hook(); } // To start listening.

   [Event]
   public void OnShootEvent(ref ShootEvent @event){
      // Handle
   }
}

public static class SomeEventHandler{

   [Event(order: 2)]
   public static void OnShootFireBullets(ref ShootEvent @event){  // ref, none, in supported and all types as a event. Only one param!
      // Do stuff
   }
   
   [Event(order: 1)]
   public static void OnShootFireBullets(ref ShootEvent @event){  // ref, none, in supported and all types as a event. Only one param!
      // Do stuff
   }
}

var shootEvent = new ShootEvent();
EventBus.Send(ref shootEvent); // Broadcasts the event to all annotated methods wherever they are.
```

The `order` is optional and determines the **order** of the calls. However, this is separate, the static methods are always called first. An order can be specified. And then come the instances that are called, within which an order can also be determined.

{% hint style="warning" %}
The call will forward the events to the receivers directly and without delay. The assignment happens via the **parameters**, so if we pass the `struct`, the methods that receive this struct as a parameter are always called. The `struct` is therefore virtually our event.
{% endhint %}
