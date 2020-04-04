# func 函数

### func函数（适用于method）小结

##### Go函数
数也是一种类型，可以和其他类型一样被保存在变量中。和接口一样，接口也是一种类型。
```
package main
import (
    "fmt"
)
func foo() {
    fmt.Println("foo")
}
func main() {
    var f func()
    f = foo
    f()
}

```

##### Go function Return value alias considerations

```
func foo(a, b int) (c, d int) {
    c = a / b 
    d = a % b
    return
}
```

* 适用于简单的函数体
* 和调用者无关，仅限函数体内，合理使用函数返回值命名，进来避免参数和返回值函数别名混用。

##### Go函数类型实现接口——把函数作为接口来调用
函数体实现接口  
函数的声明不能直接实现接口，需要将函数定义为类型后，使用类型实现结构体。当类型方法被调用时，还需要调用函数本体。
```
// 函数定义为类型
type FuncCaller func(interface{}) // func(interface{}) 定义为 FuncCaller 类型。
// 实现Invoker的Call
func (f FuncCaller) Call(p interface{}) {
    // 调用f()函数本体
    f(p) 
    //FuncCaller 的 Call() 方法将实现 Invoker 的 Call() 方法。
    // FuncCaller 的 Call() 方法被调用与 func(interface{}) 无关，还需要手动调用函数本体。
}

//代码备注：
//以上函数类型，需要函数本身进行逻辑处理。FuncCaller 无须被实例化，只需要将函数转
//换为 FuncCaller 类型即可，函数来源可以是命名函数、匿名函数或闭包。

```

##### HTTP实现场景：
函数 handler() 转为 HandlerFunc 类型，HandlerFunc 类型实现了 Handler 的 ServeHTTP 方法，  
底层可以同时使用各种类型来实现 Handler 接口进行处理。
```
HTTP 包中包含有 Handler 接口定义，代码如下：
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
Handler 用于定义每个 HTTP 的请求和响应的处理过程。

同时，也可以使用处理函数实现接口，定义如下：
type HandlerFunc func(ResponseWriter, *Request)
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
    f(w, r)
}
要使用闭包实现默认的 HTTP 请求处理，可以使用 http.HandleFunc() 函数，函数定义如下：
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
    DefaultServeMux.HandleFunc(pattern, handler)
}
而 DefaultServeMux 是 ServeMux 结构，拥有 HandleFunc() 方法，定义如下：
func (mux *ServeMux) HandleFunc(pattern string, handler func
(ResponseWriter, *Request)) {
    mux.Handle(pattern, HandlerFunc(handler))
}
上面代码将外部传入的函数 handler() 转为 HandlerFunc 类型，HandlerFunc 类型实现了 Handler 的 ServeHTTP 方法，  
底层可以同时使用各种类型来实现 Handler 接口进行处理。

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


