# pointer 指针


### Golang 中不存在引用传参，全部是值传递

```
package main

import "fmt"

func main() {
	var a int
	var b, c = &a, &a
	fmt.Println(b, c)   // 0xc0000160d0 0xc0000160d0
	fmt.Println(&b, &c) // 0xc00000e028 0xc00000e030
}
```

小结：
* 在 Golang 项目中不可能存在两个变量共享一块内存，但是可以创建两个变量指向相同的内存地址，但这和两个变量共享一块内存是不一样的。
* Golang传参全部是值传递，值拷贝或者用参数生成一个新的指针地址指向传入参数的值。即值拷贝或者通过另外一个指针操作同一个参数的值。
* Map 和 Channel 不是引用。

### 基于指针对象的方法

当调用一个函数时，会对其每一个参数值进行拷贝，如果一个函数需要更新一个变量，或者函数的其中一个参数实在太大我们希望能够避免进行这种默认的拷贝，这种情况下我们就需要用到指针了。对应到我们这里用来更新接收器的对象的方法，当这个接受者变量本身比较大时，我们就可以用其指针而不是对象来声明方法，如下：
```
func (p *Point) ScaleBy(factor float64) {
    p.X *= factor
    p.Y *= factor
}
```
这个方法的名字是(*Point).ScaleBy。这里的括号是必须的；没有括号的话这个表达式可能会被理解为*(Point.ScaleBy)。

在现实的程序里，一般会约定如果Point这个类有一个指针作为接收器的方法，那么所有Point的方法都必须有一个指针接收器，即使是那些并不需要这个指针接收器的函数。我们在这里打破了这个约定只是为了展示一下两种方法的异同而已。

只有类型(Point)和指向他们的指针(*Point)，才是可能会出现在接收器声明里的两种接收器。此外，为了避免歧义，在声明方法时，如果一个类型名本身是一个指针的话，是不允许其出现在接收器中的，比如下面这个例子：
```
type P *int
func (P) f() { /* ... */ } // compile error: invalid receiver type
```
想要调用指针类型方法(*Point).ScaleBy，只要提供一个Point类型的指针即可，像下面这样。
```
r := &Point{1, 2}
r.ScaleBy(2)
fmt.Println(*r) // "{2, 4}"
```
或者这样：
```
p := Point{1, 2}
pptr := &p
pptr.ScaleBy(2)
fmt.Println(p) // "{2, 4}"
```
或者这样:

```
p := Point{1, 2}
(&p).ScaleBy(2)
fmt.Println(p) // "{2, 4}"
```
不过后面两种方法有些笨拙。幸运的是，go语言本身在这种地方会帮到我们。如果接收器p是一个Point类型的变量，并且其方法需要一个Point指针作为接收器，我们可以用下面这种简短的写法：
```
p.ScaleBy(2)
```
编译器会隐式地帮我们用&p去调用ScaleBy这个方法。这种简写方法只适用于“变量”，包括struct里的字段比如p.X，以及array和slice内的元素比如perim[0]。我们不能通过一个无法取到地址的接收器来调用指针方法，比如临时变量的内存地址就无法获取得到：
```
Point{1, 2}.ScaleBy(2) // compile error: can't take address of Point literal
```
但是我们可以用一个*Point这样的接收器来调用Point的方法，因为我们可以通过地址来找到这个变量，只要用解引用符号*来取到该变量即可。编译器在这里也会给我们隐式地插入*这个操作符，所以下面这两种写法等价的：
```
pptr.Distance(q)
(*pptr).Distance(q)
```
这里的几个例子可能让你有些困惑，所以我们总结一下：在每一个合法的方法调用表达式中，也就是下面三种情况里的任意一种情况都是可以的：

不论是接收器的实际参数和其接收器的形式参数相同，比如两者都是类型T或者都是类型*T：
```
Point{1, 2}.Distance(q) //  Point
pptr.ScaleBy(2)         // *Point
```
或者接收器实参是类型T，但接收器形参是类型*T，这种情况下编译器会隐式地为我们取变量的地址：
```
p.ScaleBy(2) // implicit (&p)
```
或者接收器实参是类型*T，形参是类型T。编译器会隐式地为我们解引用，取到指针指向的实际变量：
```
pptr.Distance(q) // implicit (*pptr)
```
如果命名类型T(译注：用type xxx定义的类型)的所有方法都是用T类型自己来做接收器(而不是*T)，那么拷贝这种类型的实例就是安全的；调用他的任何一个方法也就会产生一个值的拷贝。比如time.Duration的这个类型，在调用其方法时就会被全部拷贝一份，包括在作为参数传入函数的时候。但是如果一个方法使用指针作为接收器，你需要避免对其进行拷贝，因为这样可能会破坏掉该类型内部的不变性。比如你对bytes.Buffer对象进行了拷贝，那么可能会引起原始对象和拷贝对象只是别名而已，但实际上其指向的对象是一致的。紧接着对拷贝后的变量进行修改可能会有让你意外的结果。

译注： 作者这里说的比较绕，其实有两点：

不管你的method的receiver是指针类型还是非指针类型，都是可以通过指针/非指针类型进行调用的，编译器会帮你做类型转换。
在声明一个method的receiver该是指针还是非指针类型时，你需要考虑两方面的内部，第一方面是这个对象本身是不是特别大，如果声明为非指针变量时，调用会产生一次拷贝；第二方面是如果你用指针类型作为receiver，那么你一定要注意，这种指针类型指向的始终是一块内存地址，就算你对其进行了拷贝。熟悉C或者C艹的人这里应该很快能明白。
