# defer usage

### defer Default execution order

The defer method is an in - and out-stack structure.
` ` `
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
` ` `

### defer Aborts execution

The defer stack operation after return will not be ignored
` ` `
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
` ` `

### defer

** * Avoid using this type of operation as it is not recommended because it is not readable **
* If the return value is not declared as a specific variable, defer will not change the return value variable

` ` `
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
` ` `

### defer summary

* defer function value will also hold the parameter value when **pushes the stack, not when executed**.

* defer is the structure statement **added** when the function executes. It is the default, not the specified structure statement.

* defer is used to ensure that the function call executes later in the program execution, usually for cleanup. Defer is often used where it is used in other languages, such as ensure and finally.

* defer in Go is like the C++ destructor; the Go area of C++ "destructors" is for functions, not objects.

* Defer can be used with the lock to ensure the release of the lock, but it should be noted that this will prolong the release time of the lock.

* Try to pay attention to the following operations:
* Do not use defer in the loop, unless you are really sure about the defer workflow.
* Avoid modifying the function error return value in defer. The error function value can be modified directly in the return of the program. Defer modifying the program return value error is generally used in collocation with recover. In other cases, it is not recommended to modify in defer to avoid abuse.

* If the function of the defer expression is defined after panic, the function will not be executed after panic.