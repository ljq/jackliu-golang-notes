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

### defer 小结

* defer 函数值在**入栈的时候也会保存参数的值，并非在执行时取值**。

* defer 是函数执行时**添加**的结构语句，为缺省项，不是指定的结构语句。

* defer 用于确保函数调用在程序执行的后期执行，通常用于清理。Defer通常用于其他语言中使用的地方，例如ensure和finally。

* Go语言中defer类似于C++中的析构函数，Go语言区域于C++“析构“中针对的的是函数，而不是对象。
