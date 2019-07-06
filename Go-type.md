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

###### 类型可比较性

| 类型 | 说明 |
| :---:| :------------------: |
| map | 宕机错误，不可比较 |
| 切片（[]T）| 宕机错误，不可比较|
| 通道（channel）| 可比较，必须由同一个 make 生成，也就是同一个通道才会是 true，否则为 false |
| 数组（[容量]T）| 可比较，编译期知道两个数组是否一致 |
| 结构体 | 可比较，可以逐个比较结构体的值 |
| 函数 | 可比较 |
