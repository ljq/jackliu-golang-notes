#  channel


### 要点

* channle 本质数据结构管道队列，数据是先进先出。
* 具有**线程安全机制**，多个go程访问时，不需要枷锁，也就是说channel本身是线程安全的。
* channel是有类型的。
* 已关闭通道注意事项：
 - 1.对一个关闭的通道再发送值就会导致panic。
 - 2.对一个关闭的通道进行接收会一直获取值直到通道为空。
 - 3.对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
 - 4.关闭一个**已经关闭**的通道会导致panic。

### channel 遍历模式

* 简单操作：data:=<-ch （如果读多次，需要用循环）
```
var chNo = make(chan int, 6)
 
func ChEach() {
	for i := 0; i < 10; i++ {
		chNo <- 8 * i
	}
 
}
func main() {
	go ChEach()
	for i:=0;i<100;i++{
		fmt.Print(<-chNo, "\t")
	}
}

```
注：
（1）如果读的次数多于写的次数会发生：fatal error: all goroutines are asleep - deadlock! ，
若close(chNo) ），多于的次数读到的数据为0（数据默认值）。
（2）读的次数<=写的次数，会读取出次数对应的内容(原则上读写次数相等)，不会报错。



* 断言方式 ```if  value, ok := <-ch; ok == true {}```
    -  如果写端没有写数据，也没有关闭。<-ch; **会阻塞**
    -  如果写端写数据， value 保存 <-ch 读到的数据。 ok 被设置为 true
    -  如果写端关闭。 value 为数据类型默认值。ok 被设置为 false

* range 方式```for num := range ch {}```

```
var chNo = make(chan int, 6)
 
func mm1() {
	for i := 0; i < 10; i++ {
		chNo <- 8 * i
	}
    //关闭，否则无法编译
	close(chNo)
}
func main() {
 
	go mm1()
	for {
		for data := range chNo {
			fmt.Print(data,"\t")
		}
		break
	}
}
```

**注意使用for-range时，遍历的变量赋值指针地址是不变的！可通过循环体中声明新变量，解决特殊场景的处理！**

遍历方式注意事项：  
总结：通过以上验证，为了保证程序的健壮性，在设计程序时，最好将channel的读、写分别在子go程中进行。写完数据之后，记得关闭channel，这样避免阻塞导致主程崩溃，同时捕捉子程错误信息。

* channel不像文件一样需要经常去关闭，只有当你确实没有任何发送数据了，或者你想显式的结束range循环之类的，才去关闭channel；
* 关闭channel后，无法向channel 再发送数据(引发 panic 错误后导致接收立即返回零值)；
* 关闭channel后，可以继续从channel接收数据；
* 对于nil channel，无论收发都会被阻塞。

