# go Slice: Essence

### Array
Go slicing is an abstract data type on top of arrays, so you must understand arrays before you understand slicing.
Array types are defined by designation and length and element type.
Arrays do not need to be explicitly initialized; array elements are automatically initialized to zero values:
Go's arrays are value semantics. An array variable represents the entire array. It is not a pointer to the first element (such as an array in C). When an array variable is assigned or passed, the entire array is actually copied. (In order to avoid copying the array, you can pass a pointer to the array, but the array pointer is not an array.) You can think of an array as a special struct. The field names of the structure correspond to the index of the array and the number of members is fixed.

#### Slice
Although arrays have their applications, they are not flexible enough, so they are not used much in Go code. However, slicing is used quite extensively. Slicing is built on arrays, but provides greater functionality and convenience.
The type of the slice is [] T, where T is the type of the slice element. Unlike arrays, slices have no fixed length.
The literal value of a slice is similar to the literal value of an array, but the slice does not specify the number of elements:
Slices can be created with the built-in function make, and the function signature is:
func make ([] T, len, cap) [] T
T represents the type of slice element being created. The function make accepts a type, a length, and an optional capacity parameter. When make is called, an array is allocated internally, and then the corresponding slice of the array is returned.
When the capacity parameter is ignored, it defaults to the specified length. The following is concise:
s: = make ([] byte, 5)
You can use the built-in functions len and cap to get slice length and capacity information.
len (s) == 5
cap (s) == 5
### The relationship between length and capacity.
A zero-valued slice type variable is nil. For zero-valued slice variables, both len and cap will return 0.
Slices can also be generated based on existing slices or arrays. The range of the slicing is specified by two semi-open intervals corresponding to the indices separated by colons.
The start and end indexes of the slice are optional; they default to zero and the length of the array, respectively.

### The essence of slicing
A slice is a description of an array's cutting interval. It contains a pointer to the array, the length of the cutting interval, and the capacity (the maximum length of the cutting interval).
A slice does not copy the entire slice element. It creates a new slice performing the same underlying array. This makes slicing operations as efficient as array indexes. Therefore, modifying elements with a new slice will also affect the original slice.
A slice cannot grow beyond its capacity. Increasing beyond the capacity of the slice will cause a runtime exception, just as the index of a slice or array is out of range causing an exception. Also, you cannot use an index less than zero to access elements before the slice.
Section growth (copy and append)
To increase the capacity of a slice, you must create a new, larger slice and then copy the contents of the original slice to the new slice. The entire technique is a common implementation that supports dynamic array languages.
The copy operation in a loop can be replaced by the copy built-in function. The copy function copies the elements of the source slice to the destination slice. It returns the number of copied elements.
func copy (dst, src [] T) int
The copy function supports copying between slices of different lengths (it only copies elements of the smallest slice length). In addition, the copy function correctly handles cases where the source and destination slices overlap.
But most programs don't require full control, so Go provides a built-in function append for most situations; its function signature:
func append (s [] T, x ... T) [] T
The append function appends x to the end of the slice s and increases the capacity if necessary.
If you want to append a slice to the end of another slice, you need to use the ... syntax to expand the second parameter into a parameter list.
You can declare a zero-valued slice (nil) and append data to the slice in a loop:

Possible "traps"
The slicing operation does not copy the underlying array. The array of this layer will be kept in memory until it is no longer referenced. Sometimes a small memory reference may cause all data to be saved.