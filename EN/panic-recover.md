# panic recover usage


### Summary

* Panic can be called directly, or it can be caused by a program runtime error.

* Recover is a built-in function to recover from panic. **recover can only play a real role in the function of defer**.

* When no panic occurs, calling recover will return nil without any effect.

* Recover must be defined in the defer statement before panic. When the panic is triggered, the goroutine will not simply terminate, but will execute the defer defined before it (in turn, take the defer and push the stack value to execute).

* Panic and recover can accept any type of value, because it is defined as interface{}:
     * func panic(v interface{})
     * func recover() interface{}
     * panic(value) => recover() => value(**Finally captured by recover**)