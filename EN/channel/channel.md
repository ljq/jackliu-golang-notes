# channel


### Essentials

* Channle essential data structure pipeline queue, data is first-in first-out.
* With ** thread safety mechanism **, when multiple go programs are accessed, no shackles are needed, which means that the channel itself is thread safe.
* channel are typed.
* Notes for closed channel:
  - 1. Sending another value on a closed channel causes a panic.
  - 2. Receiving a closed channel will get the value until the channel is empty.
  - 3. Performing a receive operation on a closed channel with no value will result in a zero value of the corresponding type.
  - 4. Closing a closed ** channel causes panic.

### channel traversal mode

* Simple operation: data: = <-ch (if read many times, need to use loop)
```
var chNo = make (chan int, 6)
 
func ChEach () {
for i: = 0; i <10; i ++ {
chNo <-8 * i
}
 
}
func main () {
go ChEach ()
for i: = 0; i <100; i ++ {
fmt.Print (<-chNo, "\ t")
}
}

```
Note:
(1) If there are more reads than writes: fatal error: all goroutines are asleep-deadlock!,
If close (chNo)), the data read more than times is 0 (default value of data).
(2) The number of reads <= the number of writes, the content corresponding to the number of reads will be read (in principle, the number of reads and writes is equal), and no error will be reported.



* Assertion method ```if value, ok: = <-ch; ok == true ()` ``
    -If the writer does not write data, it is not closed. <-ch; ** will block **
-If the write end writes data, value saves <-ch the read data. ok is set to true
-If the write end is closed. value is the default value of the data type. ok is set to false

* range method ```for num: = range ch ()` ``

```
var chNo = make (chan int, 6)
 
func mm1 () {
for i: = 0; i <10; i ++ {
chNo <-8 * i
}
    // Close, otherwise it will not compile
close (chNo)
}
func main () {
 
go mm1 ()
for {
for data: = range chNo {
fmt.Print (data, "\ t")
}
break
}
}
```

** Note that when using for-range, the traversed variable assignment pointer address is unchanged! You can solve the special scenarios by declaring new variables in the loop body! **

Notes on traversal method:
Summary: Through the above verification, in order to ensure the robustness of the program, when designing the program, it is best to read and write the channel separately in the sub-go process. After writing the data, remember to close the channel, so as to avoid blocking and cause the main program to crash, and at the same time capture the subroutine error information.

* channel do not need to be closed as often as files. Only when you do not have any data to send, or you want to explicitly end the range loop, etc., do you close the channel;
* After closing the channel, it is impossible to send data to the channel again (after a panic error is caused, the receiver immediately returns zero);
* After closing the channel, you can continue to receive data from the channel;
* For nil channel, no matter sending or receiving, it will be blocked.