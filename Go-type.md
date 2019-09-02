# type关键词总结

### type 用法：

* 定义结构体
* 定义接口 
* 类型别名 
* 类型定义
* 类型开关

类型定义和类型别名的区别：  
区别就是类型定义是完全定义了一种新的类型，
而类型别名只是给现有的类型取了一个别名alias。（编译器会替换成基本类型。）


### Type常见Demo

###### 定义结构体
```
type Demo struct {}

```

###### 定义接口
```
type Demoer interface {}

```

###### 类型别名
```
type Demo string

```

###### 类型定义
```
type handle func(str string) 

```

###### 类型开关
```
func Demo(params ...interface{})  {
   for i, x := range params {
      switch x.(type) {
      case bool:
         fmt.Printf("type #%d is bool",i)
      default:
         fmt.Printf("type is unknow")
      }
   }
}

```

### 类型注意事项

###### 类型比较
备注：
参考Go文档type说明： 
* 命名类型（简单类型），有类型名称如 int, int64, float, string, bool. 还有自定义的命名类型。 
* 非命名类型（复杂类型），没类型名称 array slice, map，func(){}, interface{}。但是**chan类型是可以==比较**。
   * slice 内存不连续，底层对象分开放的，不能直接比较； 
   * map 内存不连续，底层对象独立存放，不能直接比较；
   * chan 内存是连续的，单一对象，可以直接比较；
* 当比较两个命名类型的时候，类型名称必须一样；当比较命名类型和非命名类型的时候，**底层类型一样即可**。
* 比较基于两个原则：1.内存底层基本类型；2.类型本身是否确定类型或不稳定类型；

* 空接口值比较

```
package main

import (
	"fmt"
	"reflect"
)

type T1 []string
type T2 []string

func main() {
	foo0 := []string{}
	foo1 := T1{}
	foo2 := T2{}
	fmt.Println(reflect.TypeOf(foo0))
	fmt.Println(reflect.TypeOf(foo1))
	fmt.Println(reflect.TypeOf(foo2))

	// Output:
	// []string
	// main.T1
	// main.T2

	//编译通，反之亦然
	//foo1 = foo0
	//foo0 = foo1

	// 编译不通过
	// 错误提示：cannot use foo2 (type T2) as type T1 in assignment
	foo2 = foo0
   foo1 = foo2 //cannot



   //编译通过：chan
   ch1 := make(chan int)
   ch2 := make(chan int)
   fmt.Println(ch1 == ch2)   
    
   //编译不通过：slice
   s1 := []int{1,2}
   s2 := []int{2,1}
   fmt.Println(s1 == s2)

   //编译不通过：map
   m1 := make(map[int]int)
   m2 := make(map[int]int)
   fmt.Println(m1 == m2)
}

```


###### 类型可比较性

判断原则：
底层数据结构类型是否稳定和一致

| 类型 | 说明 |
| :--- | :------------------ |
| map | 宕机错误，不可比较 |
| 切片（[]T）| 宕机错误，不可比较|
| 通道（channel）| 可比较，必须由同一个 make 生成，也就是同一个通道才会是 true，否则为 false |
| 数组（[容量]T）| 可比较，编译期知道两个数组是否一致 |
| 结构体 | 可比较，可以逐个比较结构体的值 |
| 函数 | 可比较 |
