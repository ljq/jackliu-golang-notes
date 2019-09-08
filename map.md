# Map 要点

### 注意事项

* map 是引用类型的： 内存用 make 方法来分配。
* new，永远用 make 来构造 map。new() 分配了一个引用对象，你会获得一个空引用的指针，相当于声明了一个未初始化的变量并且取了它的地址。
* 当 map 增长到容量上限的时候，如果再增加新的 key-value 对，map 的大小会自动加 1。所以出于性能的考虑，对于大的 map 或者会快速扩张的 map，即使只是大概知道容量，也最好先标明。

### 特殊用法

用切片作为 map 的值
```
mp1 := make(map[int][]int)
mp2 := make(map[int]*[]int)
```

只遍历键KEY时，使用下面的形式,无须将值改为匿名变量形式，忽略值即可:  
```
for key := range mapData {
    //Code ...
}
```

map清空：
无相关函数和方法。截至Go 1.12，清空唯一办法就是重新 make 一个新的 map。  
但担心垃圾回收的效率，Go的GC的回收效率远高于一个清空函数。

### map并发安全

###### sync.Map有以下特性：

* 无须初始化，直接声明即可。
* sync.Map 不能使用 map 的方式进行取值和设置等操作，而是使用 sync.Map 的方法进行调用。
    * Store 表示存储，
    * Load 表示获取，
    * Delete 表示删除。
* 遍历操作 Range加回调函数，回调函数返回内部遍历出来的值。
* Range 参数中的回调函数的返回值功能是：
    * 需要继续迭代遍历时，返回 true；
    * 终止迭代遍历时，返回false。

```
#遍历所有sync.Map中的键值对
mapData.Range(func(k, v interface{}) bool {
    fmt.Println("list-kv: %v , %v", k, v)
    return true
})

```