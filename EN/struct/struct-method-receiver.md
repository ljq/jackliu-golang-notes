# About Go: Structure (Method)-Method (Receiver) type applicable scenario selection and naming convention (Go official suggestion)


### When to use value type scenarios
1. If the recipient is a map, func or chan, use value types (because they are reference types themselves).
2. If the receiver is a slice and the method does not perform a reslice operation, nor does it reallocate memory to the slice, use a value type.
3. If the receiver is a small array or a native value type structure type (such as time.Time type), and there are no fields and pointers that can be modified, or the receiver is a simple basic type like int and string , Use value types just fine.
A value type receiver can reduce a certain amount of garbage generation. If a value is passed into a value type receiver method, a copy on the stack will replace the allocation of memory on the heap (but not guaranteed to be successful), so in Before you figure out what the code is trying to do, don't choose a value type acceptor for this reason.



### Use pointer type scene
1. If the method needs to modify the receiver, the receiver must be a pointer type.
2. If the receiver is a struct containing sync.Mutex or similar synchronization fields, the receiver must be a pointer to avoid copying.
3. If the receiver is a large structure or array, then the pointer type receiver is more efficient.
4. Can the recipient be modified when functions and methods are called concurrently from this method? A receiver of a value type creates a copy when the method is called, so external modifications cannot be applied to this receiver. If the modification must be visible to the original recipient, then the recipient must be of pointer type.
5. If the receiver is a structure, array or slice, any of them is a pointer type and may be modified, it is recommended to use a pointer type receiver, which will increase the readability of the program.



### Receiver receiver name
1. The community-appointed recipient name is a one- or two-letter abbreviation for the type (like c or cl for Client).
2. Avoid using generic names like me, this or self, and avoid using overly descriptive names;
3. If you use c in one place, don't use cl in other places;



### Self-Summary
In general usage scenarios, decide whether to use pointers, and see the size of the capacity of the data monomer (structure, interface, etc ...) (Special attention: pay attention to the slice type, dictionary map, and channel channel itself reference type, the underlying itself is the pointer call ), Whether the form of data monomer storage at the language level is itself a pointer type, the scope of the data monomer and the scope of operation, in the final analysis, it is necessary to plan the data structure in advance according to the actual specific business scenario, and consider the data monomer in the development. Whether the cost of copying is too high.
The official recommendations of Go are also a summary of the principles of Go memory allocation and data storage.
Although the Go language has pointers, it does not include pointer calculations, and the operation of pointers is very simple. (This point is particularly good. The simple structure allows the GC to monitor the running status of memory at the lowest cost, avoiding excessive pointer associations, which is beneficial to GC memory management And recycling).