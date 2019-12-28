# func

### func（Apply to method）

##### Go function
A number is also a type that can be stored in a variable like any other type. Like interfaces, interfaces are a type.

```
package main
import (
    "fmt"
)
func foo() {
    fmt.Println("foo")
}
func main() {
    var f func()
    f = foo
    f()
}

```

##### The Go function type implements the interface -- calls the function as an interface
The function body implements the interface
The declaration of a function does not directly implement the interface; you need to define the function as a type and then use the type to implement the structure. When a type method is called, the function ontology needs to be called as well.
` ` `
The // function is defined as a type
Type FuncCaller func(interface{}) // func(interface{}) is defined as type FuncCaller.
// implements the Invoker Call
Func (f FuncCaller) Call(p interface{}) {
// calls the f() function ontology
F (p)
The Call() method of //FuncCaller implements the Call() method of Invoker.
The Call() method of // FuncCaller is called regardless of func(interface{}), and the function ontology needs to be called manually.
}

// code remarks:
The above function types require the function itself to be logically processed. The FuncCaller doesn't need to be instantiated; it just needs to be turned around
The function source can be a named function, an anonymous function, or a closure.

```

##### HTTP implementation scenario:
The function handler() is converted to the type HandlerFunc, which implements the handler's ServeHTTP method,
The underlying layer can use various types simultaneously to implement the Handler interface for processing.

```
The HTTP package contains the Handler interface definition. The code is as follows:
Type Handler interface {
ServeHTTP (ResponseWriter, * Request)
}
The Handler is used to define the processing of each HTTP request and response.

At the same time, the interface can also be implemented by using the handler function, which is defined as follows:
Type HandlerFunc func (ResponseWriter, * Request)
Func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
F (w, r)
}
To use closures for the default HTTP request handling, you can use the http.handlefunc () function, which is defined as follows:
Func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
DefaultServeMux. HandleFunc (pattern, handler)
}
DefaultServeMux is the ServeMux structure with the HandleFunc() method, defined as follows:
Func(mux *ServeMux) HandleFunc(pattern string, handler func)
(ResponseWriter, * Request)) {
Mux. Handle (pattern, HandlerFunc (handler))
}
The above code turns the incoming function handler() into the type HandlerFunc, which implements the handler's ServeHTTP method,
The underlying layer can use various types simultaneously to implement the Handler interface for processing.

```


##### Go variable argument function call notes (three dots)

* if the string array is sliced and each item is passed to the function separately, the []T{arg1,arg2} and other similar operations will be performed and initialized again;
* if the interface{} array slices are scattered and directly passed into the function, the object is no longer created, and the object is directly tried;

# # # the Go... Three dots usage:

Variable length function arguments (argument by argument)
Call the last parameter... T, using several parameters of type T at the end of the argument list.
. The type of T inside the function is actually []T.

#### calls a function that has a variable length argument list
The slice type parameter does not have to split the call, it is directly followed by slice...

Identifies the number of elements in the array
For example, [...]. Int {1, 2, 3}

Wildcards on the command line
Describes the wildcard character of the package file. Executes all packages of the current directory and all subdirectories:

```

go test ./...
go run ./...
go build ./..

```


