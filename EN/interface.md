# interface interface

### go: interface {}, assertions, and type conversions
* Go's interface is more useful for declaring a collection of methods than for type constraints.
* interface {} can be used to pass a variable of any type to a function, but for a function, the variable is still an interface {} type (empty interface type), so it must be checked after the type assertion is confirmed (cannot be implicitly converted directly) ).
* The conversion of interface types to ordinary types is called type assertion (runtime determination).

### Basic Principles of Interface Conversion

* Common type = "Interface type: implicit conversion by the compiler at runtime.
* Interface type = "common type: must be an explicit type assertion.
* Superset and subset conversion relationship: Superrotor can, subrotation can't.

###### Assertion recommendation method:
```

b, ok: = a. ([] int)
if ok {
    ...
}

// Assertion failure will not report an error during the compilation phase, so it is likely that an assertion failure will cause a runtime error.
```

* 1. The role of assertions: when using interface {}, solve the type conversion problem of empty interface type conversion to ordinary type;
* 2. Conversion between ordinary types, use explicit type conversion, otherwise the consequences are severe and uncontrollable.


### Interface value passing considerations:
* If the interface implements the method and the type's own implementation uses a value receiver, then either a pointer or a value can be used when passing the value.
* If the interface implements a method and the type's own implementation uses a pointer receiver, then the address must be passed when passing the value.
```
 Reason: The compiler cannot automatically obtain an undeclared address.

 The method defined by the structure type can be called by the pointer type of the structure; when the structure type calls the method of the pointer type, it is converted into a pointer, not directly called.
 When an interface implements a method, an interface function implemented with a pointer type can only be considered as a pointer type implementation, and a method implemented with a structure type is also implemented as a pointer type.

```

### interface {} and [] interface {}

```
var dataSlice [] int = foo ()
var interfaceSlice [] interface {} = dataSlice
```
**Compile Error**  
```
cannot use dataSlice (type [] int) as type [] interface {} in assignment
```
Assign any type to interface {}, you cannot assign any type of slice to [] interface {}
 ** Can't ** Assign some [] MyType slices directly to [] interface {}, the data behind them represent different meanings.

 ```
 //Compile Error
// t: = [] int {1, 2, 3, 4} wrong
// var s [] interface {} = t

//correct
t: = [] int {1, 2, 3, 4} // right
s: = make ([] interface (}, len (t))
for i, v: = range t {
 s [i] = v
}
 ```
###### Interface Conversion
* Use type inference to determine whether an interface object is a specific interface or type.
* You can also use switch for batch type judgment. Fallthrough is not supported.
* Superset interface objects can be converted to subset interfaces, otherwise errors occur.


###### the reason:
* [] interface {} type ** Not ** interface {} type, it is a slice, and the type of the slice element is exactly interface {}.
* [] interface {} type variables have a specific memory structure, which is determined at compile time. Each interface {} occupies two words, one word is used to store the type of the interface, and the other word is used to store the actual data or a pointer to the data. So behind the [] interface {} type slice of length N is a piece of data of N * 2 word length.
This is different from a normal [] MyType type slice. The size of the data block behind a [] MyType slice of the same length is N * sizeof (MyType).

###### How to use:
If you want to get a container with a list of any type, and convert it to the original data type before indexing the elements in it, you can directly use interface {}. This approach is very general (if ** not compile-time typesafe) and fast.


### Interface Type Memory Layout (Principle)

interface is actually composed of two members in memory
* Tab points to Virtual Table
* data points to the actually referenced data.
* The virtual table depicts the actual type information and the set of methods required by the interface.


The underlying structure of the interface
runtime.h
```
struct Iface
{
Itab * tab;
void * data;
};

struct Itab
{
InterfaceType * inter;
Type * type;
void (* fun []) (void);
};

struct Itab
{
InterfaceType * inter;
Type * type;
void (* fun []) (void);
};

// The interface is equal to nil only if both tab and data are nil.
```

Interface Demo:
```
package main

import (
"fmt"
)

type People interface {
Do ()
}

type Student struct {
UserId int
UserName string
}

func (s Student) Do () {
s.UserName = "Jack Liu"
return
}

func main () {
stu: = Student {1, "Jack"}
p: = People (stu) //
//a.Do ()
fmt.Printf ("% T% v", p, p)
}

```

The People interface itself, the bottom layer contains two parts, the tab virtual table and the data actually stored in the value;
```p: = People (Student {1," Jack "}) // This is legal` ``, the essence:
** Function call through interface **, the actual operation is actually ```p.tab-> fun [0] (p.data)` ``;

###### Refer to the similarities and differences of virtual tables in Go and C ++:
* C ++:
    * C ++ virtual table is generated at compile time. Note: the polymorphism is determined at runtime;
    * Each class creates a method set (virtual table);
    * When the subclass overrides the virtual function of the parent class, the corresponding function pointer in the table is changed to a function implemented by the subclass itself;
    * If not, point to the implementation of the parent class;
    * When faced with multiple inheritance, there will be multiple virtual table pointers in the C ++ object structure, and each virtual table pointer points to a different part of the method set.
* Go:
    * The virtual table of the Go interface is generated at runtime;
    * ```p: = People (Student {1," Jack "})` `` generates a virtual table with a People interface corresponding to the Student type and caches it.
###### the reason:
* Go has no inheritance relationship and uses a combination method, so virtual table initialization cannot be performed (how many types implement a certain interface, and how many interfaces a single type implements, which makes the compiler unable to know.
* It is natural to choose to generate a virtual table at runtime. When running at runtime, you only need to analyze whether the type implements all the methods of the interface when the interface is needed. This avoids the need to maintain a large number of inheritance and binding relationships. Mental burden, this does not cause too much performance problems.
* Go interface combination scheme and C ++ do the opposite, in essence, each has advantages and disadvantages.



### Interface Tips
Let the compiler check to make sure that a type implements the interface.
```var _ fmt.Stringer = (* Data) (nil)` ``

In some cases, it can save a lot of time to have a function "implement" an interface directly.
```
type Tester interface {
Do ()
}

type FuncDo func ()
func (self FuncDo) Do () {self ()}

func main () {
var t Tester = FuncDo (func () {println ("Hello, World!")})
t.Do ()
}

```