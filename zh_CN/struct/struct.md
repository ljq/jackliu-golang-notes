# struct

# 结构体地址：
    结构体变量的指针地址 == 结构体首个元素的指针地址。

# 结构体序列号注意事项

golang利用gob序列化struct对象保存到本地:
**务必注意的是golang使用gob```"encoding/gob"```序列化有个小坑,就是struct里的字段必须要可导出也就是首字母大写**
gob编码序列化struct对象保存到本地的应用场景我想是小应用需要保存数据在本地(比如配置文件等),这样就不需要用到
sql数据库,可以简单部署应用.

### 面向对象Class类的底层实现从某些方面说就是结构体，对象的引用就是指针，只是语言把他们封装起来了而已。

Golang的Struct结构体（源于C语言，但又有别于C）的灵活性：
go语言中并没有像C++，Java语言中这类的Class，它只含有像C语言中的结构体，用结构体和指针等特性，完成一个类的作用，很巧妙的使用了指针和结构体，不仅是go的面向对象，包括go语言中的map等操作都是借助了结构体。其实，说白了，C++、Java等面向对象的语言中，类的底层实现就是结构体，对象的引用就是指针，只是语言把他们封装起来了而已。很多人刚接触面向对象很不理解这些东西也应该缘于此。
或者说，面向对象的封装在某种意义上是以牺牲灵活特性的为代价的一种抽象简化。

所有高级语言(PHP、Java、Python、JavaScript等等...)的数组Array、字典map、Slice切片、Json等结构类型往往只是叫法不一样，多少都源于C系或者受到C系的Struct结构体思想的影响,在某些特定领域内，做了一些针对性的解决方案。

PHP就是c语言实现的一套高级“程序”语言，只不过是这套“程序化的语言”的规范和语法等机制可以用来快速做web领域的事情，通过解释器，转成底层语言完成代码的最终执行。

这也印证了很多答案往往追根溯源都在计算机数据结构基础里没有捷径可以走。

### golang struct注意事项：

对于struct类型来说，字段的先后顺序是非常关键的。如果两个struct类型包含了完全相同的字段，但是排列顺序不同或者进行了部分合并，那么这两个struct就是不同的类型！

如果struct字段是大写字母开头，那么该字段就是导出的（包外可见），这也符合Go语言的可见性规则。因此一个struct可以同时包含导出和未导出的变量。

### struct{} 空结构体

* struct{} 特点
struct{}是一个无元素的结构体类型，通常在没有信息存储时使用。优点是大小为0，不需要内存来存储struct {}类型的值。
* struct{} 和 struct{}{} 区别
struct{}{}是一个复合字面量(literal)，它构造了一个struct {}类型的值，该值也是空。
* 声明为声明为map[string]struct{}
由于struct{}是空，不关心内容，这样map便改造为set
map可以通过“comma ok”机制来获取该key是否存在,例如_, ok := map["key"],如果没有对应的值,ok为false。
* chan struct{}：可以用作通道的退出
* 两个structt{}{}地址相等

```
var set map[string]struct{}
// Initialize the set
set = make(map[string]struct{})

// Add some values to the set:
set["one"] = struct{}{}
set["two"] = struct{}{}

// Check if a value is in the map:
_, ok := set["one"]

//output true
fmt.Println("Is one in the map?", ok)
_, ok = set["three"]

//output  false
fmt.Println("Is three in the map?", ok)
```


