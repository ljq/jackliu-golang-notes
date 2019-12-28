# chain operation


### Notes for chaining calls
* itself is a broader concept
* chained call scenario:
* the return value is a function or method (rarely practical);
* method chain (func chain or method chain) : when the return value is an object, you can directly call the method bound by the object;
* chain call method chain is an inclusion relation, strictly speaking, chain call *≠* method chain;

### Benefits of chaining calls
* the calling process is closer to natural language, and the chain rendering is readable;
* the method of parameter list complexity is greatly simplified and clear;
* simplify the amount of code to avoid repeated writing of the main operation object;
* advantages: design, operation and data separation, decoupling;

### Disadvantages of chaining calls
* developers need to keep in mind the number of layers being called, not too many.
* each call requires the creation of a deep call stack, which is relatively inefficient.

### The essence of the chain operation
(function OR method) operation = "return object =" continue operation return object belongs to the function OR method

```
package main

import "fmt"

type A struct {
    Aa string
    Ab  int
}

func (p *A) SetAa(aa string) *A {
    p.Aa = aa
    return p //Return operation object
}

func (p *A) SetAb(ab int) *A {
    p.Ab = ab
    return p //Return operation object
}

func (p *A) Print()  {
    fmt.Printf("Aa:%s || Ab:%d\n", p.Aa, p.Ab)
}

func main() {
    varA := &A{}
    varA.SetAa("Aa").SetAb(100).Print()
}

```
