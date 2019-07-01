### func函数（适用于method）小结

##### Go函数
数也是一种类型，可以和其他类型一样被保存在变量中。和接口一样，接口也是一种类型。
```
package main
import (
    "fmt"
)
func foo() {
    fmt.Println("fire")
}
func main() {
    var f func()
    f = foo
    f()
}

```

##### Go函数类型实现接口——把函数作为接口来调用
函数体实现接口  
函数的声明不能直接实现接口，需要将函数定义为类型后，使用类型实现结构体。当类型方法被调用时，还需要调用函数本体。
```
// 函数定义为类型
type FuncCaller func(interface{})
// 实现Invoker的Call
func (f FuncCaller) Call(p interface{}) {
    // 调用f()函数本体
    f(p)
}
//代码说明如下：
//第 2 行，将 func(interface{}) 定义为 FuncCaller 类型。
//第 5 行，FuncCaller 的 Call() 方法将实现 Invoker 的 Call() 方法。
//第 8 行，FuncCaller 的 Call() 方法被调用与 func(interface{}) 无关，还需要手//动调用函数本体。
//以上函数类型，需要函数本身进行逻辑处理。FuncCaller 无须被实例化，只需要将函数转换为 ///FuncCaller 类型即可，函数来源可以是命名函数、匿名函数或闭包
```



##### Go可变参数函数调用注意事项（three dots）

* 若将 string 数组切片，每项独自传入函数，则会执行[]T{arg1,arg2}等类似操作，从新进行初始化；
* 若将 interface{} 数组切片打散后直接传入函数，则不再进行对象的创建，直接试用该对象；

### Go ... three dots 用法：

##### 变长的函数参数（逐一传参用法）
调用参数最后一个...T，在参数列表的最后使用若干个类型为T的参数。
...T在函数内部的类型实际是[]T.

##### 调用拥有变长参数列表的函数
slice类型参数不必拆分调用，直接在slice后跟...

##### 标识数组元素个数
例如[...]int{1,2,3}

##### Go命令行中的通配符
描述包文件的通配符。会执行当前目录和所有子目录的所有包：

```

go test ./...
go run ./...
go build ./..

```


