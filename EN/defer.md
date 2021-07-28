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

# # # defer summary

* defer is used to **ensure** that a function call is performed later in a program's execution, usually for purposes of cleanup. defer is often used where e.g. ensure and finally would be used in other languages.

* defer in Go is like the C++ destructor; the Go area of C++ "destructors" is for functions, not objects.
The * defer is the structure statement ** added when the function executes. It is the default, not the specified structure statement.