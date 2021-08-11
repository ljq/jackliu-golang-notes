# struct

# Structure address:
    The pointer address of the structure variable == the pointer address of the first element of the structure.

# Notes on structure serial number

Golang uses gob to serialize struct objects and save them locally:
**It is important to note that golang uses gob```"encoding/gob"``` serialization has a small pit, that is, the fields in the struct must be exportable, that is, the first letter is capitalized**
Gob encoding serialized struct objects are saved locally. I think small applications need to save data locally (such as configuration files, etc.), so that they do not need to be used
sql database, you can easily deploy applications.

### The underlying implementation of the object-oriented Class is a structure in some respects, and object references are pointers, but the language encapsulates them.

The flexibility of Golang's Struct structure (derived from the C language, but different from C):
Go language does not have classes like C++ and Java language. It only contains structures like C language. It uses structures and pointers to complete the role of a class. It uses pointers and structures very cleverly. , Not only the object-oriented of go, but also operations such as map in the go language all use the structure. In fact, to put it plainly, in object-oriented languages ​​such as C++ and Java, the underlying implementation of a class is a structure, and object references are pointers, but the language encapsulates them. Many people who are new to object-oriented do not understand these things and it should be due to this.
In other words, object-oriented encapsulation is, in a sense, an abstract simplification at the expense of flexibility.

All high-level languages ​​(PHP, Java, Python, JavaScript, etc...) structure types such as Array, dictionary map, Slice slice, Json, etc. are often just called differently, and some of them are derived from the C series or are influenced by the C series of Struct The influence of structural thinking has made some targeted solutions in certain specific fields.

PHP is a set of high-level "program" languages ​​implemented by the C language. It's just that the specifications and grammar of this "programmed language" can be used to quickly do things in the web field, through an interpreter, and turn it into a low-level language. The final execution of the code.

This also confirms that many answers are often traced to the source, and there is no shortcut in the foundation of computer data structure.

### golang struct notes:

For the struct type, the order of the fields is very critical. If two struct types contain exactly the same fields, but are arranged in a different order or partially merged, then the two struct types are different types!

If the struct field starts with a capital letter, then the field is exported (visible outside the package), which also complies with the visibility rules of the Go language. Therefore, a struct can contain both exported and unexported variables.

### struct{} empty structure

* struct{} features
struct{} is a structure type with no elements, usually used when there is no information storage. The advantage is that the size is 0 and no memory is needed to store the value of type struct {}.
* Declared as declared as map[string]struct{}
Since struct{} is empty and does not care about the content, map is transformed into set
The map can obtain the existence of the key through the "comma ok" mechanism, such as _, ok := map["key"], if there is no corresponding value, ok is false.
* chan struct{}: Can be used as the exit of the channel

### struct{}{} compound literal’s Notes

* struct {}
struct {} is a **structure type with no elements**, usually used when there is no information storage.
The advantage：no memory is needed to store the value of struct{} type.
* struct{}{}
struct{}{} is a **compound literal**, which constructs a value of **struct{} type**, which is also empty.
* Two structt{}{} addresses are equal

```
package main

import "fmt"

type idBval struct {
Id int
}

func main() {
idA := struct{}{}
fmt.Printf("idA: %T and %v \n\n", idA, idA)

idB := idBval{
		1,
}
idB.Id = 2
fmt.Printf("idB: %T and %v \n\n", idB, idB)

idC := struct {
Id int
}{
		1,
}
fmt.Printf("idC: %T and %v \n\n", idC, idC)

mapD := make(map[string]struct{})
mapD["mapD"] = struct{}{}
_, ok := mapD["mapD"]
fmt.Printf("mapD['mapD'] is %v \n\n", ok)

sliceE := make([]interface{}, 2)
sliceE[0] = 1
sliceE[1] = struct{}{}
fmt.Printf("idE: %T and %v \n\n", sliceE, sliceE)

}

```

Output:
```

idA: struct {} and {}

idB: main.idBval and {2}

idC: struct {Id int} and {1}

mapD['mapD'] is true

idE: []interface {} and [1 {}]

```
