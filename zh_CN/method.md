# method 小结

### 语法糖：

#### 非语法糖
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
    
    
    //调用方式：
    fmt.Println(a.Name())

    //值传递，值拷贝（非语法糖）
    fmt.Println(A.Name(a))

}
```

#### 非语法糖 指针接收者实际操作
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
    
    //调用方式：对应操作的接收者
    fmt.Println(a.Name())

    //值传递，指针地址拷贝（非语法糖）
    fmt.Println( (*A).Name(a) )

}
```

#### 语法糖：

```
a.Name()转换为(*a).Name()
```



**语法糖转换是在编译期发生。**


