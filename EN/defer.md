# defer usage

### defer Default execution order

The defer method is an in - and out-stack structure.
```
package main

import(
		"fmt"
)

func main(){
	defer func(){
	fmt.Println("1")
	} ()
	defer func(){
	fmt.Println("2")
	} ()
	defer func(){
	fmt.Println("3")
	} ()

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

### defer Aborts execution

The defer stack operation after return will not be ignored
```
package main

import(
	"fmt"
)

func main(){
	defer func(){
	fmt.Println("1")
	} ()
	defer func(){
	fmt.Println("2")
	} ()

	return

	defer func(){
	fmt.Println("3")
	} ()
	defer func(){
	fmt.Println("4")
	} ()
}

// output
// 2
// 1
```

### defer

***Avoid using this type of operation as it is not recommended because it is not readable.**
* If the return value is not declared as a specific variable, defer will not change the return value variable

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

// Function body: name return value
func testA()(data string) {
	data = "OLD"
	defer func(){
	data = "NEW"
	} ()
	return res
}

// Function body: non-name return value
func testB()(string) {
	data := "OLD"
	defer func(){
	data = "NEW"
	} ()
	return res
}

// output
// NEW
// OLD
```

### Precautions for defer usage scenarios
***Although defer is suitable for resource release scenarios, it is not suitable for lock release scenarios.**
Try not to use defer to release lock. Although the code logic and mentality related to lock can be reduced in the subsequent code logic, this method will cause the entire function to be locked. If there is a lot of complicated or waiting, blocking logic behind, it will lead to too long lock holding time, occupying a large amount of resources and affecting performance.
* The idea of optimization is to split the code logic or functions as much as possible, try to let lock control only some shared resources, and put aside the defer to control the unlock, avoiding lock granularity and holding time for too long, resulting in unnecessary resource consumption.

### defer summary

* defer function value will also hold the parameter value when**pushes the stack, not when executed**.

* defer is the structure statement**added**when the function executes. It is the default, not the specified structure statement.

* defer is used to ensure that the function call executes later in the program execution, usually for cleanup. Defer is often used where it is used in other languages, such as ensure and finally.

* defer in Go is like the C++ destructor; the Go area of C++ "destructors" is for functions, not objects.

* Defer can be used with lock to ensure the release of lock, but it should be noted that this will prolong the release time of lock.

* Try to pay attention to the following operations:
* Do not use defer in the loop, unless you are really sure about the defer workflow.
* Avoid modifying the function error return value in defer. The error function value can be modified directly in the return of the program. Defer modifying the program return value error is generally used in collocation with recover. In other cases, it is not recommended to modify in defer to avoid abuse.

* If the function of the defer expression is defined after panic, the function will not be executed after panic.

***Although defer is suitable for resource release scenarios, it is not suitable for lock release scenarios.**