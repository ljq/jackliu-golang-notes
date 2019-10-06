# chain operation


### 链式调用注意事项
* 本身为较宽泛的概念
* 链式调用的场景：
    * 返回值是一个函数或方法(很少有实际意义)；
    * 方法链（func chain或method chain）:返回值是一个对象时，可以直接调用对象绑定的方法；
* 链式调用方法链是包含关系，严格意义上说，链式调用​ *≠* 方法链；

### 链式调用的优点
* 调用过程更接近自然语言，链式呈现可读性好；
* 参数列表复杂化的方法极大简化清晰；
* 精简代码量，避免主操作对象反复的书写；
* 优势:设计上，操作与数据分离、解耦;

### 链式操作的实质
（函数OR方法）操作 =》 返回对象 =》继续操作返回对象所属函数OR方法

```
package main

import "fmt"

type A struct {
    Aa string
    Ab  int
}

func (p *A) SetAa(aa string) *A {
    p.Aa = aa
    return p //返回操作对象
}

func (p *A) SetAb(ab int) *A {
    p.Ab = ab
    return p //返回操作对象
}

func (p *A) Print()  {
    fmt.Printf("Aa:%s || Ab:%d\n", p.Aa, p.Ab)
}

func main() {
    varA := &A{}
    varA.SetAa("Aa").SetAb(100).Print()
}

```
