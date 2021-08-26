# defer 用法

### defer 默认执行顺序

defer 方法**执行顺序**是先进后出的栈入栈出结构。
```
package main

import(
	"fmt"
)

func main(){
	defer func(){
		fmt.Println("1")
	}()
	defer func(){
		fmt.Println("2")
	}()
	defer func(){
		fmt.Println("3")
	}()

	fmt.Println("4")
	fmt.Println("5")

	return
}

// output
// 4
// 5
// 3
// 2
// 1
```

### defer 中止执行

return之后的defer栈操作都不会被忽略
```
package main

import(
	"fmt"
)

func main(){
	defer func(){
		fmt.Println("1")
	}()
	defer func(){
		fmt.Println("2")
	}()

	return

	defer func(){
		fmt.Println("3")
	}()
    defer func(){
		fmt.Println("4")
	}()
}

// output
// 2
// 1
```

### defer修改返回值注意事项

* **尽量避免使用此类操作，可读性不强不推荐**
* 返回值如果是非声明具体变量，则defer不会修改返回值变量

```
package main

import(
	"fmt"
)

func main(){
	da := testA()
	fmt.Println(da)

    db := testB()
	fmt.Println(db)
}

// 函数体：命名式返回值
func testA()(data string) {
	data = "OLD"
	defer func(){
		data = "NEW"
	}()
	return res
}

// 函数体：非命名式返回值
func testB()(string) {
	data := "OLD"
	defer func(){
		data = "NEW"
	}()
	return res
}

// output
// NEW
// OLD
```

### defer使用场景注意事项
* **defer虽然适用在资源释放的场景，但不适合锁的释放场景。**
尽量不用 defer 释放锁，虽然在后续的代码逻辑可以减少锁相关的代码操作逻辑和心智，但此方式会导致整个函数被锁住。如果后面还有很多复杂或者等待、阻塞的逻辑等等，会导致锁持有时间过长，占用较大资源影响性能。
* 优化的思路：是尽量分拆代码逻辑或函数，尽量只让锁控制一些共享资源，抛开defer自行控制unlock，避免锁粒度过大和持有时间过长，造成不必要的资源消耗。

### defer 小结

* defer 函数值在**入栈的时候也会保存参数的值，并非在执行时取值**。

* defer 是函数执行时**添加**的结构语句，为缺省项，不是指定的结构语句。

* defer 用于确保函数调用在程序执行的后期执行，通常用于清理。Defer通常用于其他语言中使用的地方，例如ensure和finally。

* Go语言中defer类似于C++中的析构函数，Go语言区域于C++“析构“中针对的的是函数，而不是对象。

* defer可以配合锁的使用来确保锁的释放,但要注意此会延长锁的释放时间。

* 尽量注意以下操作：
	* 不要在循环里面使用defer，除非你真的确定defer的工作流程
	* 避免defer中修改函数error返回值，error函数值可以直接在程序的return中修改，defer修改程序返回值error一般用在和recover搭配中。其他情况下不建议defer中修改，避免滥用。

* defer 表达式的函数如果定义在 panic 后面，该函数在panic后则不会执行。

* **defer虽然适用在资源释放的场景，但不适合锁的释放场景。**