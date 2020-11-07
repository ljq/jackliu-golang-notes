# method summary

### Syntactic sugar:

#### Non-syntactic sugar
```
type A struct {
     name string
}

func (a A) Name() string {
     a.name = "a"
     return a.name
}

func main() {
     a:= A{name:"aaa"}
    
    
     //Calling method:
     fmt.Println(a.Name())

     //Value transfer, value copy (non-syntactic sugar)
     fmt.Println(A.Name(a))

}
```

#### Non-syntactic sugar, pointer receiver actual operation
```
type A struct {
     name string
}

func (a *A) Name() string {
     a.name = "a"
     return a.name
}

func main() {

     a:= &A{name:"aaa"}
    
     //Calling method: the receiver of the corresponding operation
     fmt.Println(a.Name())

     //Value transfer, pointer address copy (non-syntactic sugar)
     fmt.Println( (*A).Name(a))

}
```

#### Syntactic sugar:

```
a.Name() is converted to (*a).Name()
```


**Syntax sugar conversion occurs at compile time. **