### go：interface{}、断言与类型转换
interface{}可用于向函数传递任意类型的变量，但对于函数内部，该变量仍然为interface{}类型（空接口类型），故必须进行类型断言确认类型后检查才能使用（不能直接隐式转换）。接口类型向普通类型的转换称为类型断言(运行期确定)。  

###### 断言推荐方式：
```

b,ok:=a.([]int)
if ok{
    ...
}

//断言失败在编译阶段不会报错，故很可能出现断言失败导致运行错误。
```

* 1.断言的作用：使用interface{}时，解决空接口类型向普通类型转换的类型转换问题；
* 2.普通类型之间的转换，使用显式的类型转换，否则后果严重不可控。


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

 ```
 //编译错误
//t := []int{1, 2, 3, 4} wrong 
//var s []interface{} = t 

//正确
t := []int{1, 2, 3, 4} //right 
s := make([]interface{}, len(t)) 
for i, v := range t { 
 s[i] = v 
}
 ```
###### 接口转换
* 利用类型推断，可判断接口对象是否某个具体的接口或类型。  
* 还可用 switch 做批量类型判断，不支持 fallthrough。  
* 超集接口对象可转换为子集接口，反之出错。  


###### 原因：
* []interface{}类型 **不是** interface{}类型， 它是一个切片，切片元素的类型恰好是interface{}。
* []interface{}类型变量拥有特定的内存结构，这在编译时就已经决定。每个interface{}占两个字（word)，一个字用于存放interface存放的类型，另一个字用于存放实际数据或者是指向数据的指针。于是长度为N的[]interface{}类型切片背后是一个N*2字长的一块数据。   
这与一般的[]MyType类型切片不同，相同长度的[]MyType切片背后的数据块大小为N*sizeof(MyType)字长。   

###### 使用方式：
如果想得到一个元素为任意类型的列表的容器，并且在索引其中元素之前会把它转换为原本的数据类型，可以直接使用interface{}即可。此种方式很通用（如果 **不是编译时类型安全** 的）也很快速。   


### 接口类型内存布局(原理)

interface在内存上实际由两个成员组成
* tab指向虚表(Virtual Table)
* data则指向实际引用的数据。
* 虚表描绘了实际的类型信息及该接口所需要的方法集。


接口的底层结构
runtime.h
```
struct Iface
{
	Itab* tab;
	void* data;
};

struct Itab
{
	InterfaceType* inter;
	Type* type;
	void (*fun[])(void);
};

struct Itab
{
	InterfaceType* inter;
	Type* type;
	void (*fun[])(void);
};

//只有 tab 和 data 都为 nil 时，接口才等于 nil。
```

接口Demo：
```
package main

import (
	"fmt"
)

type People interface {
	Do()
}

type Student struct {
	UserId   int
	UserName string
}

func (s Student) Do() {
	s.UserName = "Jack Liu"
	return
}

func main() {
	stu := Student{1, "Jack"}
	p := People(stu)//
	//a.Do()
	fmt.Printf("%T %v", p, p)
}

```

People接口本身，底层含有tab虚表和data实际存储的值两部分；    
```p := People(Student{1, "Jack"})//此是合法的```，本质：    
**通过接口进行函数调用** ，实际的操作其实就是```p.tab->fun[0](p.data)```;  

###### 参考Go和C++的虚表的异同：
* C++：
    * c++ 的虚表是在编译时生成的，注意：表现出的多态是在runtime运行时决定;
    * 每个class创建了一个方法集(虚表);
    * 当子类重写父类的虚函数时，就将表中的相应函数指针改为子类自己实现的函数;
    * 如果没有则指向父类的实现;
    * 当面临多继承时，C++对象结构里就会存在多个虚表指针，每个虚表指针指向该方法集的不同部分。
* Go:
    * Go 接口的虚表是在runtime运行时生成；
    * ``` p := People(Student{1, "Jack"})```生成People接口对应于Student类型的虚表，并将其缓存。
###### 原因：
* Go无继承关系，采用的是组合方式，所以不能进行虚表初始化(多少类型实现了某个接口，单个类型到底实现了多少接口这让编译器无从获知.
* 选择在运行时生成虚表是自然的方案，放到runtime运行时，只要在需要接口的去分析一下类型是否实现了接口的所有方法即可，这样避免了去维护大量继承和绑定关系的心智负担，此并不会带来性能上的太大问题。
* Go接口组合的方案和C++反其道而行之，本质上来说，各有优缺点。



### 接口技巧
让编译器检查，以确保某个类型实现接口。
``` var _ fmt.Stringer = (*Data)(nil) ```

某些时候，让函数直接 "实现" 接口能省不少事。
```
type Tester interface {
	Do()
}

type FuncDo func()
func (self FuncDo) Do() { self() }

func main() {
	var t Tester = FuncDo(func() { println("Hello, World!") })
	t.Do()
}

```
