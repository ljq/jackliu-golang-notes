### buildin func

```
Append          -- used to append elements to arrays and slices, and return modified arrays and slices
Close           -- mainly used to close the channel
Delete          -- delete the value corresponding to the key from the map
Panic           -- Stop the regular goroutine (panic and recover: used for error handling)
Recover         -- Allows the program to define the panic action of goroutine
Imag            -- returns the real part of complex (complex, real imag: used to create and operate complex numbers)
Real            -- returns the imaginary part of complex
Make            -- used to allocate memory and return the Type itself (only applicable to slice, map, channel)
New             -- Used to allocate memory, mainly used to allocate value types, such as int and struct. Returns a pointer to the Type
Cap             -- capacity means capacity, which is used to return the maximum capacity of a type (only for slices and maps)
Copy            -- used to copy and connect slices, returns the number of copies
Len             -- to calculate the length, such as string, array, slice, map, channel, and return the length
Print, println  -- the underlying print function. It is recommended to use the fmt package in the deployment environment
```