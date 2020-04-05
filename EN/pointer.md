# pointer


### Golang there is no reference to pass parameters, all are value pass

```
package main

import "fmt"

func main() {
	var a int
	var b, c = &a, &a
	fmt.Println(b, c)   // 0xc0000160d0 0xc0000160d0
	fmt.Println(&b, &c) // 0xc00000e028 0xc00000e030
}
```
Summary:
* it is impossible for two variables to share a block of memory in a Golang project, but it is possible to create two variables pointing to the same memory address, which is not the same as two variables sharing a block of memory.
* Golang is all value passing, value copying or parameter generating a new pointer address to the value of the incoming parameter. Copy the value or manipulate the value of the same parameter through another pointer.
* Map and Channel not reference.

### Pointer-based methods

When a function is called, each parameter value is copied. If a function needs to update a variable, or one of the function's parameters is too large, we hope to avoid such a default copy. In this case, we will Need to use pointers. Corresponding to the method we use to update the receiver object here, when the receiver variable itself is relatively large, we can use its pointer instead of the object to declare the method, as follows:

func (p * Point) ScaleBy (factor float64) {
    p.X * = factor
    p.Y * = factor
}
The name of this method is (* Point) .ScaleBy. The parentheses are required here; without the parentheses this expression might be interpreted as * (Point.ScaleBy).

In practical programs, it is generally agreed that if the Point class has a pointer method as a receiver, all Point methods must have a pointer receiver, even those functions that do not need this pointer receiver. We broke this convention here just to show the similarities and differences between the two methods.

Only types (Point) and pointers (* Point) to them are the two receivers that may appear in the receiver declaration. In addition, in order to avoid ambiguity, when declaring a method, a type name is not allowed to appear in the receiver if it is a pointer itself, such as the following example:

type P * int
func (P) f () {/ * ... * /} // compile error: invalid receiver type
To call the pointer type method (* Point) .ScaleBy, just provide a pointer of type Point, as shown below.

r: = & Point {1, 2}
r.ScaleBy (2)
fmt.Println (* r) // "{2, 4}"
Or this:

p: = Point {1, 2}
pptr: = & p
pptr.ScaleBy (2)
fmt.Println (p) // "{2, 4}"
Or this:

p: = Point {1, 2}
(& p) .ScaleBy (2)
fmt.Println (p) // "{2, 4}"
But the latter two methods are a bit awkward. Fortunately, the go language itself will help us in such places. If the receiver p is a variable of type Point, and its method requires a Point pointer as the receiver, we can use the following short form:

p. ScaleBy (2)
The compiler will implicitly help us call the ScaleBy method with & p. This shorthand method only applies to "variables", including fields in structs such as p.X, and elements in arrays and slices such as perim [0]. We can't call the pointer method through a receiver that cannot get the address. For example, the memory address of the temporary variable cannot be obtained:

Point {1, 2} .ScaleBy (2) // compile error: can't take address of Point literal
But we can use a receiver like * Point to call Point's method, because we can find this variable by address, as long as we use the dereference symbol * to get the variable. The compiler will also implicitly insert the * operator here, so the following two expressions are equivalent:

pptr.Distance (q)
(* pptr) .Distance (q)
Here are a few examples that may confuse you a bit, so let's summarize: in each legal method call expression, that is, any of the following three cases is possible:

Both the actual parameters of the receiver and the formal parameters of the receiver are the same, for example, both are type T or both type * T:

Point {1, 2} .Distance (q) // Point
pptr.ScaleBy (2) // * Point
Or the receiver argument is type T, but the receiver parameter is type * T, in which case the compiler implicitly takes the address of the variable for us:

p.ScaleBy (2) // implicit (& p)
Or the receiver argument is type * T and the formal parameter is type T. The compiler will implicitly dereference us and get the actual variable pointed to by the pointer:

pptr.Distance (q) // implicit (* pptr)
If all methods of the named type T (type defined by type xxx) use the type T as a receiver (not * T), then it is safe to copy an instance of this type; call any The method also produces a copy of the value. For example, this type of time.Duration will be copied when the method is called, including when it is passed as a parameter to the function. But if a method uses a pointer as a receiver, you need to avoid copying it, because this might break the invariance inside the type. For example, if you copy the bytes.Buffer object, it may cause the original object and the copied object to be just aliases, but the objects they point to are actually the same. Modifying the copied variables immediately afterwards may have unexpected results.

Translator's note: The author here is more circumstantial, in fact there are two points:

Regardless of whether your method receiver is a pointer type or a non-pointer type, it can be called through a pointer / non-pointer type, and the compiler will help you do the type conversion.
When declaring whether a receiver of a method is a pointer or a non-pointer type, you need to consider two aspects. The first aspect is whether the object itself is particularly large. If it is declared as a non-pointer variable, the call will generate a copy; The second aspect is that if you use a pointer type as a receiver, then you must pay attention that this pointer type always points to a memory address, even if you copy it. Those familiar with C or C 艹 should quickly understand here.