# struct

### The underlying implementation of the object-oriented Class class is in some ways a structure, and references to objects are pointers, but the language encapsulates them.

The flexibility of Golang's Struct structure (originally derived from C, but different from C):
There is no Class like C ++ or Java in Go. It only contains structures like C. It uses structs and pointers to complete the role of a class. It uses pointers and structures very cleverly. Not only is Go's object-oriented, but also operations such as map in the Go language rely on structures. In fact, to put it bluntly, in object-oriented languages ​​such as C ++ and Java, the underlying implementation of a class is a structure, and a reference to an object is a pointer, just the language encapsulates them. Many people who are new to object-oriented and do not understand these things should also be due to this.
In other words, object-oriented encapsulation is, in a sense, an abstract simplification at the expense of flexible features.

Arrays, dictionary maps, slice slices, Json and other structural types in all high-level languages ​​(PHP, Java, Python, JavaScript, etc ...) are often called differently, and they all originate from the C system or the Struct of the C system. The influence of structural thinking has made some targeted solutions in some specific areas.

PHP is a set of high-level "programming" languages ​​implemented by the C language, but it is just a set of "programmatic languages" with specifications and syntax mechanisms that can be used to quickly do things in the web domain. They can be converted to the underlying language by interpreters The final execution of the code.

This also confirms that many answers often go back to the roots of computer data structure without shortcuts.

### golang struct Notes:

For struct types, the order of the fields is critical. If two struct types contain exactly the same fields, but in a different order or are partially merged, then the two structs are different types!

If the struct field starts with a capital letter, then the field is exported (visible outside the package), which also conforms to the visibility rules of the Go language. So a struct can contain both exported and unexported variables.