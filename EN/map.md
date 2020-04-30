# Map Essentials

### Precautions

* map is a reference type: memory is allocated using the make method.
* new, always use make to construct maps. new () allocates a reference object and you get a null reference pointer, which is equivalent to declaring an uninitialized variable and taking its address.
* When the map grows to the maximum capacity, if a new key-value pair is added, the size of the map will automatically increase by 1. So for the sake of performance, for large maps or maps that will expand rapidly, it is best to indicate them even if you only know the capacity.

### Special usage

Use slices as map values
```
mp1: = make (map [int] [] int)
mp2: = make (map [int] *[] int)
```

When only traversing the key KEY, use the following form, without changing the value to the anonymous variable form, ignore the value:
```
for key: = range mapData {
    // Code ...
}
```

map clear:
No related functions and methods. As of Go 1.12, the only way to clear is to remake a new map.
But worrying about the efficiency of garbage collection, Go's GC's recovery efficiency is much higher than an emptying function.

### map Concurrency Security Considerations

#### sync.Map has the following characteristics:

* No need to initialize, just declare it.
* sync.Map can't use map to get values ​​and settings. Instead, use sync.Map method to call.
    * Store means storage,
    * Load means get,
    * Delete means delete.
* Traversal operation Range plus callback function, the callback function returns the value traversed internally.
* The return value function of the callback function in the Range parameter is:
    * It needs to return true when iterating and iterating is needed;
    * When terminating the iteration, it returns false.

```
#definition
var mapData sync.Map


#Iterate over all key-value pairs in sync.Map
mapData.Range (func (k, v interface{}) bool {
    fmt.Println ("list-kv:% v,% v", k, v)
    return true
})

```
