# reflect reflection

### reflect package type definition

```
// reflect package type definition

type Kind Sign

const (
    Invalid Kind = iota
    Bool // Boolean
    Int // signed integer
    Int8 // signed 8-bit integer
    Int16 // signed 16-bit integer
    Int32 // signed 32-bit integer
    Int64 // signed 64-bit integer
    Uint // unsigned integer
    Uint8 // unsigned 8-bit integer
    Uint16 // unsigned 16-bit integer
    Uint32 // unsigned 32-bit integer
    Uint64 // unsigned 64-bit integer
    Uintptr // pointer
    Float32 // single-precision floating-point number
    Float64 // double-precision floating-point number
    Complex64 // 64-bit complex type
    Complex128 // 128-bit complex number type
    Array // array
    Chan // channel
    Func // function
    Interface // interface
    Map // map
    Ptr // pointer
    Slice // slice
    String // string
    Struct // structure
    UnsafePointer // underlying pointer
)

```

### Reflected structure TypeOf underlying structure:
```

func TypeOf(i interface{}) Type {
     eface := *(*emptyInterface)(unsafe.Pointer(&i)) // runtime.eface
     return toType(eface.type)
}

type Type interface {
     Align() int
     FieldAlign() int
     Method(int) Method
     MethodByName(string) (Method, bool)
     NumMethod() int
     Name() string
     PkgPath() string
     Size() uintptr
     String() string
     Kind() Kind //Kind Sign
     Implements(u Type) bool
     AssignableTo(u Type) bool
     ConvertibleTo(u Type) bool
     Comparable() bool
     //... more fields
}

```

### General principles of reflection performance

The general principles of numerical analysis of benchmark results suggest:
* Try to avoid reflection when using native code.
* Buffering reflection value objects in advance can greatly help performance.
* Avoid reflection function calls. When you really need to call, buffer the function parameter list in advance, and use the return value as little as possible.

### Usage scenario of reflection
* The main implementation of gRPC is through reflection.
* Inversion of Control, Dependency Injection
* Type assertion: determine the actual type of the interface variable at runtime
