# panic recover 用法


### 要点小结

* panic可以直接调用，也可以是程序运行时错误导致。

* recover是一个从panic恢复的内建函数。**recover只有在defer的函数里面才能发挥真正的作用**。

* 没有发生panic时，调用recover将会返回nil并且没有任何影响。

* recover必须定义在panic之前的defer语句中。当panic被触发时，该goroutine不会简单的终止，而是会执行在它之前定义的defer(依次取defer压栈值执行)。

* Panic和recover可以接受任何类型的值，因为定义为interface{}：
    * func panic(v interface{})
    * func recover() interface{}
    * panic(value) => recover() => value(**最终由recover捕获**)
