# 方法值 & 方法表达式

### 方法值



```
package main

import "fmt"

type Person struct {
    name string
    sex byte
    age int
}

func (p Person) SetInfoValue ()  {
    fmt.Printf("SetInfoValue: %p, %v\n", &p, p)
}

func (p *Person) SetInfoValuePointer ()  {
    fmt.Printf("SetInfoValuePointer: %p, %v\n", p, p)
}

func main() {
    p := Person{"mike", 'm', 18}
    fmt.Printf("main: %p, %v\n", &p, p)
    p.SetInfoValuePointer()//传统调用方式
    pFunc := p.SetInfoValuePointer //这个就是方法值，调用函数时，无需再传递接收者，隐藏了接收者
    pFunc() //等价于p.SetInfoPointer()
    
    vFunc := p.SetInfoValue  //这里完成内容拷贝的操作
    vFunc()
}
```