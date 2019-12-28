# func value & func expression

###
Functions or methods have two characteristics:
* 1. The function itself is a type T;
* 2. Function variables: functions or methods themselves are stored in variables;

### value method
The essence of method value is that the receiver is hidden when called, which is equivalent to the traditional call.

```
package main

import "fmt"

type A struct {
    name string
}

func (a *A) SetValue ()  {
    fmt.Println("set")
}

func main() {
    aa := A{"jack"}
    //Traditional call
    aa.SetValue()

    //Method values that hide the receiver when invoked
    vFunc := aa.SetValue
    vFunc()
	
    //Method expressions that hide the receiver when called
    aFunc := (*A).SetValue

    //Pass the receiver explicitly, equivalent to aa.setvalue()
    aFunc(&aa)
}

```