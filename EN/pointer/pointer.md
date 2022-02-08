# pointer pointer

### There is no reference parameter in Golang, all are passed by value

```
package main

import "fmt"

func main() {
	var a int
	var b, c = &a, &a
	fmt.Println(b, c) 	// 0xc0000160d0 0xc0000160d0
	fmt.Println(&b, &c) // 0xc00000e028 0xc00000e030
}
```

summary:
* It is impossible for two variables to share a piece of memory in a Golang project, but two variables can be created to point to the same memory address, but this is different from two variables sharing a piece of memory.
* Golang parameters are all passed by value, copying the value or generating a new pointer address with the parameter to point to the value of the incoming parameter. Copy by value or manipulate the value of the same parameter through another pointer.
* Map and Channel are not references.

### Pointer-based methods

When a function is called, each parameter value is copied. If a function needs to update a variable, or one of the parameters of the function is too large, we want to avoid this default copy. In this case, we will Need to use pointers. Corresponding to the method we use here to update the object of the receiver, when the receiver variable itself is relatively large, we can use its pointer instead of the object to declare the method, as follows:
```
func (p *Point) ScaleBy(factor float64) {
    p.X *= factor
    p.Y *= factor
}
```
The name of this method is (*Point).ScaleBy. The parentheses here are required; without parentheses the expression might be interpreted as *(Point.ScaleBy).

In real programs, it is generally agreed that if the Point class has a pointer as a receiver method, then all Point methods must have a pointer receiver, even those functions that do not need this pointer receiver. We've broken this convention here just to show the similarities and differences between the two approaches.

Only types ( Point ) and pointers to them ( *Point ) are the two types of receivers that may appear in a receiver declaration. In addition, to avoid ambiguity, when declaring a method, a type name that is itself a pointer is not allowed to appear in the receiver, such as the following example:
```
type P *int
func (P) f() { /* ... */ } // compile error: invalid receiver type
```
To call the pointer type method (*Point).ScaleBy, just provide a pointer of type Point, like the following.
```
r := &Point{1, 2}
r.ScaleBy(2)
fmt.Println(*r) // "{2, 4}"
```
or this:
```
p := Point{1, 2}
pptr := &p
pptr.ScaleBy(2)
fmt.Println(p) // "{2, 4}"
```
or this:

```
p := Point{1, 2}
(&p).ScaleBy(2)
fmt.Println(p) // "{2, 4}"
```
However, the latter two methods are somewhat clumsy. Fortunately, the go language itself will help us in such places. If the receiver p is a variable of type Point, and its method requires a Point pointer as the receiver, we can use the following shorthand:
```
p.ScaleBy(2)
```
The compiler will implicitly help us call the ScaleBy method with &p. This shorthand only applies to "variables", including fields in structs such as p.X, and elements in arrays and slices such as perim[0]. We can't call pointer methods through a receiver that can't get the address, such as the memory address of a temporary variable:
```
Point{1, 2}.ScaleBy(2) // compile error: can't take address of Point literal
```
But we can use a receiver like *Point to call Point's methods, because we can find the variable by its address, just use the dereference symbol * to get to the variable. The compiler also implicitly inserts the * operator for us here, so the following two ways of writing are equivalent:
```
pptr.Distance(q)
(*pptr).Distance(q)
```
Here are a few examples that might confuse you a bit, so let's summarize: In every valid method call expression, any of the following three cases is OK:

Whether the actual parameters of the receiver and the formal parameters of its receiver are the same, for example, both are of type T or both are of type *T:
```
Point{1, 2}.Distance(q) // Point
pptr.ScaleBy(2) // *Point
```
Or the receiver argument is of type T, but the receiver parameter is of type *T, in which case the compiler will implicitly take the address of the variable for us:
```
p.ScaleBy(2) // implicit (&p)
```
Or the receiver actual parameter is of type *T and the formal parameter is of type T. The compiler will implicitly dereference for us to get the actual variable pointed to by the pointer:
```
pptr.Distance(q) // implicit (*pptr)
```
If all methods of a named type T use the T type itself as a receiver (instead of *T), then it is safe to copy an instance of that type; call any of its The method also produces a copy of the value. For example, this type of time.Duration will be copied all the time when its method is called, including when it is passed as a parameter to the function. But if a method uses a pointer as a receiver, you need to avoid copying it, as doing so may break the invariance inside the type. For example, if you copy the bytes.Buffer object, it may cause that the original object and the copied object are just aliases, but in fact the objects they point to are the same. Immediately modifying the copied variable may have unexpected results for you.

Annotation: What the author is talking about here is rather convoluted, but there are actually two points:

Regardless of whether the receiver of your method is a pointer type or a non-pointer type, it can be called through a pointer/non-pointer type, and the compiler will do the type conversion for you.
When declaring whether the receiver of a method should be a pointer or a non-pointer type, you need to consider two internal aspects. The first aspect is whether the object itself is very large. If it is declared as a non-pointer variable, the call will generate a copy; The second aspect is that if you use a pointer type as the receiver, you must pay attention that this pointer type always points to a memory address, even if you copy it. Those who are familiar with C or C fuck should understand quickly here.