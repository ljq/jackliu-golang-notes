# chain-operation

### 链式操作的本质：操作完毕后把该对象一并返回

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
	//链式调用实际是：（函数OR方法）操作 =》 返回对象 =》（对象所属函数OR方法）下一步操作
    varA.SetAa("Aa").SetAb(100).Print()
}

```
