# 方法值(func value) & 方法表达式(func expression)

### 依据
函数或方法具备两大特点：
1.函数本身是一种类型T；
2.函数变量：函数或方法本身储存在变量中；

### 方法值

方法值的本质是调用时隐藏了接收者，和传统调用是等价的；



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
    //传统调用
    aa.SetValue()

    //方法值，调用时隐藏接收者
    vFunc := aa.SetValue
    vFunc()
	
    //方法表达式，调用时隐藏接收者
    aFunc := (*A).SetValue

    //显式传递接收者,等价于aa.SetValue()
    aFunc(&aa)
}

```