# new, make, struct ()

| Function | Scope | Return Value | Filled Value |
| ------- | --- | ----------- | -------: |
make () | Create type only (slice map channel) reference. When make returns a complex structure as a slice: it is a structure with 3 fields: a pointer to the first element in the slice, and the length of the slice , And the capacity of the slice. | Non-zero value, make (T, args) returns an initialized (not zeroed) value of type T (not * T). The difference is because the three types reference data structures that must be initialized before use |
new () | all types | pointers, new returns a pointer to cleared memory, and make returns a complex structure. Zero value, new (T) will be a new item of type T, but new does not initialize the memory, just resets it to zero |


#### Note: When strut is initialized directly with struct {}, a struct value is returned instead of a pointer.