### go：interface{}、断言与类型转换
interface{}可用于向函数传递任意类型的变量，但对于函数内部，该变量仍然为interface{}类型（空接口类型），故必须进行类型断言确认类型后检查才能使用（不能直接隐式转换）。


### interface值传递注意事项：
* 如果接口实现方法，类型自己的实现使用的是值接收器，那么在传递值的时候无论使用指针还是值都可以。
* 如果接口实现方法，类型自己的实现使用的是指针接收器，那么在传递值的时候必须传递地址。
```
 原因：编译器不能自动获得一个未声明地址。
```

### interface{} 与 []interface{}

```
var dataSlice []int = foo()
var interfaceSlice []interface{} = dataSlice
```
**编译错误**  
```
cannot use dataSlice (type []int) as type []interface { } in assignment  
```
任何类型赋值给interface{}，不能把任何类型的切片赋值到[]interface{}   
 **不能** 直接将某些[]MyType切片赋值给[]interface{}， 他们背后代表的数据意义不同。   

###### 原因：
* []interface{}类型 **不是** interface{}类型， 它是一个切片，切片元素的类型恰好是interface{}。但即便这样解释，有人可能仍然觉  得前面的用法没毛病。
* []interface{}类型变量拥有特定的内存结构，这在编译时就已经决定。每个interface{}占两个字（word)，一个字用于存放interface存放的类型，另一个字用于存放实际数据或者是指向数据的指针。于是长度为N的[]interface{}类型切片背后是一个N*2字长的一块数据。   
这与一般的[]MyType类型切片不同，相同长度的[]MyType切片背后的数据块大小为N*sizeof(MyType)字长。   

###### 使用方式：
如果想得到一个元素为任意类型的列表的容器，并且在索引其中元素之前会把它转换为原本的数据类型，可以直接使用interface{}即可。此种方式很通用（如果 **不是编译时类型安全** 的）也很快速。   

