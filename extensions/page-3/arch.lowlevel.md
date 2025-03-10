---
description: Arch.LowLevel, a collection of unmanaged and unsafe collections.
---

# Arch.LowLevel

`Arch.Extended` also has a number of high-performance, unsafe collections and utils. They are mostly `blittable`, `unmanaged` and use pointers directly to access memory. This has the advantage that they are fast and not affected by the GC.

Data-structures:

* `UnsafeArray<T>`
* `UnsafeList<T>`
* `UnsafeStack<T>`
* `UnsafeQueue<T>`
* `UnsafeJaggedArray<T>`
* `JaggedArray<T>`
* `SparseJaggedArray<T>`

<table><thead><tr><th>Name</th><th>Purpose</th><th width="186">Unmanaged &#x26; Blittable</th></tr></thead><tbody><tr><td><code>UnsafeArray&#x3C;T></code></td><td>An unmanaged Array.</td><td>✅</td></tr><tr><td><code>UnsafeList&#x3C;T></code></td><td>An unmanaged List.</td><td>✅</td></tr><tr><td><code>UnsafeStack&#x3C;T></code></td><td>An unmanaged Stack.</td><td>✅</td></tr><tr><td><code>UnsafeQueue&#x3C;T></code></td><td>An unmanaged Queue.</td><td>✅</td></tr><tr><td><code>UnsafeJaggedArray&#x3C;T></code></td><td>An unmanaged JaggedArray,  made out of buckets.</td><td>✅</td></tr><tr><td><code>UnsafeSparseJaggedArray&#x3C;T></code></td><td>An unmanaged SparseJaggedArray, made out of buckets. </td><td>✅</td></tr><tr><td><code>JaggedArray&#x3C;T></code></td><td>A managed JaggedArray, made out of buckets. </td><td>❌</td></tr><tr><td><code>SparseJaggedArray&#x3C;T></code></td><td>A managed SparseJaggedArray, made out of buckets. </td><td>❌</td></tr><tr><td><code>Array</code></td><td>A class that either stores its items automatically in an <code>UnsafeArray</code> or an managed <code>Array</code>. </td><td>❌</td></tr><tr><td><code>Resources</code></td><td>A class that stores managed items and returns a <code>Handle&#x3C;T></code>to reference them in components. </td><td>❌</td></tr></tbody></table>

## Examples

The use of collections is actually a matter of course. Nevertheless, let's take a look at the most important ones.

### Array

```csharp
// Create (Using automatically disposes)
using var array = new UnsafeArray<int>(8);
array[0] = 0;
array[1] = 1;

// Acess
ref var intRef = ref array[0];

// Iterate
foreach(ref var item in array){
   Console.WriteLine(item);
}    
```

### List

```csharp
// Create (Using automatically disposes)
using var list = new UnsafeList<int>(8);
list.Add(1);
list.Add(2);

// Acess
ref var intRef = ref list[0];

// Iterate
foreach(ref var item in list){
   Console.WriteLine(item);
}    
```

### Resources

Thanks to the `Resources`, you can access and use a **managed** resource from anywhere using a unique ID. This saves **memory** and is **blittable**, so it can also be used in unsafe code.

```csharp
// Create resources jagged array and create a sprite handle
var resources = new Resources<Sprite>(IntPtr.Size, capacity: 64);
var handle = resources.Add(new Sprite());
var sprite = resources.Get(in handle);

// Use handle wherever you want to access the sprite
public struct SpriteComponent{
   public Handle<Sprite> handle;
}
```
