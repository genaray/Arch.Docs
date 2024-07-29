# Optimizations

## Passing parameters to updates

There are several ways to pass parameters during [query](query.md) iteration.

DeltaTime will be used as an example.

### Clojure 
The easiest, yet the slowest. 

Notice this causes **garbage** allocation for clojure of DeltaTime each call
```csharp
World.Query(in queryDesc,
(ref Position pos, ref Velocity vel) =>
{
    pos.Value.X += vel.Value.X * DeltaTime;
    pos.Value.Y += vel.Value.Y * DeltaTime;
});
```

 Alternativly you can use static variables to pass parameters without allocation, for example using static class Time, with public property DeltaTime. 
```csharp

public static class Time
{
    public float DeltaTime { get; private set; }
}

World.Query(in queryDesc,
(ref Position pos, ref Velocity vel) =>
{
    pos.Value.X += vel.Value.X * Time.DeltaTime;
    pos.Value.Y += vel.Value.Y * Time.DeltaTime;
});
```

### Custom inline query with parameters
To pass parameters this way we use a separate struct implementing `IForEach`, which has update method, and any required parameters are set to the struct.
```csharp
private struct VelocityUpdate : IForEach<Position, Velocity>
{
    public float DeltaTime;
    
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public void Update(ref Position pos, ref Velocity vel)
    {
    pos.Value.X += vel.Value.X * DeltaTime;
    pos.Value.Y += vel.Value.Y * DeltaTime;
    }
}

var velUpdate = new VelocityUpdate
{
    DeltaTime = DeltaTime
};
World.InlineQuery<VelocityUpdate, Position, Velocity>(in queryDesc, ref velUpdate);
```

### Custom loop
Iterate `chunks` by hand, this way we can use params directly
```csharp
var queryDesc = new QueryDescription().WithAll<Position, Velocity>();
var query = World.Query(in queryDesc);

foreach (ref var chunk in query)
{
    var positions = chunk.GetSpan<Position>();
    var velocities = chunk.GetSpan<Velocity>();
    
    for (var i = 0; i < chunk.Size; i++)
    {
        ref var pos = ref positions[i];
        ref var vel = ref velocities[i];
        
        pos.Value.X += vel.Value.X * DeltaTime;
        pos.Value.Y += vel.Value.Y * DeltaTime;
    }
}
```