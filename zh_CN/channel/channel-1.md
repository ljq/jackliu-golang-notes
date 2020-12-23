#  channel 使用场景


channel的使用场景(数据流动)

* 消息传递、消息过滤
* 信号广播
* 事件订阅与广播
* 请求、响应转发
* 任务分发
* 结果汇总
* 并发控制
* 同步与异步
...

channel的基本操作和注意事项
channel存在3种状态：

* nil，未初始化的状态，只进行了声明，或者手动赋值为nil
* active，正常的channel，可读或者可写
* closed，已关闭，**千万不要误认为关闭channel后，channel的值是nil**
* channel可进行3种操作：Read读 Write写 Close关闭

| 操作 | nil的channel | 正常channel | 已关闭channel
|:--------:|:-------:|:------:|:----------------:|
| <- ch	阻塞 | 成功或阻塞 | 读到零值 |
| ch <-	阻塞 | 成功或阻塞 | panic |
| close(ch)	panic |	成功 | panic |

** 3种操作 X 3种channel状态 = 组合9种场景 **


* 1. 使用for range读channel

场景：

当需要不断从channel读取数据时。

使用for-range读取channel，这样既安全又便利，当channel关闭时，for循环会自动退出，无需主动监测channel是否关闭，可以防止读取已经关闭的channel，造成读到数据为通道所存储的数据类型的零值。

用法
```
for x := range ch{
    fmt.Println(x)
}
```

* 2. 使用v,ok := <-ch + select操作判断channel是否关闭
场景
v,ok := <-ch + select操作判断channel是否关闭

ok的结果和含义：
- `true`：读到通道数据，不确定是否关闭，可能channel还有保存的数据，但channel已关闭。
- `false`：通道关闭，无数据读到。

* 3. 使用select处理多个channel
场景
需要对多个通道进行同时处理，但只处理最先发生的channel时

select可以同时监控多个通道的情况，只处理未阻塞的case。当通道为nil时，对应的case永远为阻塞，无论读写。特殊关注：普通情况下，对nil的通道写操作是要panic的。


* 4. 使用channel的声明控制读写权限
场景
协程对某个通道只读或只写时

目的：

使代码更易读、更易维护，
防止只读协程对通道进行写数据，但通道已关闭，造成panic。

用法
如果协程对某个channel只有写操作，则这个channel声明为只写。
如果协程对某个channel只有读操作，则这个channe声明为只读。

```
chonlyread := make(<-chan int) //创建只读channel
chonlywrite := make(chan<- int) //创建只写channel
```

```
// 只有generator进行对outCh进行写操作，返回声明
// <-chan int，可以防止其他协程乱用此通道，造成隐藏bug
func generator(int n) <-chan int {
    outCh := make(chan int)
    go func(){
        for i:=0;i<n;i++{
            outCh<-i
        }
    }()
    return outCh
}

// consumer只读inCh的数据，声明为<-chan int
// 可以防止它向inCh写数据
func consumer(inCh <-chan int) {
    for x := range inCh {
        fmt.Println(x)
    }
}
```

* 5. 使用缓冲channel增强并发
场景
异步

有缓冲通道可供多个协程同时处理，在一定程度可提高并发性。
用法
```
// 无缓冲
ch1 := make(chan int)
ch2 := make(chan int, 0)
// 有缓冲
ch3 := make(chan int, 1)
// 使用5个`do`协程同时处理输入数据
func test() {
    inCh := generator(100)
    outCh := make(chan int, 10)

    for i := 0; i < 5; i++ {
        go do(inCh, outCh)
    }

    for r := range outCh {
        fmt.Println(r)
    }
}

func do(inCh <-chan int, outCh chan<- int) {
    for v := range inCh {
        outCh <- v * v
    }
}
```

* 6. 为操作加上超时
场景
需要超时控制的操作

使用select和time.After，看操作和定时器哪个先返回，处理先完成的，就达到了超时控制的效果

* 7. 使用time实现channel无阻塞读写
场景
并不希望在channel的读写上浪费时间

是为操作加上超时的扩展，这里的操作是channel的读或写
注：time.After等待可以替换为default，则是channel阻塞时，立即返回的效果

* 8. 使用close(ch)关闭所有下游协程
场景
退出时，显示通知所有协程退出

所有读ch的协程都会收到close(ch)的信号

* 9. 使用chan struct{}作为信号channel
场景
使用channel传递信号，而不是传递数据时

没数据需要传递时，传递空struct

* 10. 使用channel传递结构体的指针而非结构体
场景
使用channel传递结构体数据时

**channel本质上传递的是数据的拷贝，拷贝的数据越小传输效率越高，传递结构体指针，比传递结构体更高效**

* 11. 使用channel传递channel
场景
使用场景有点多，通常是用来获取结果。

channel可以用来传递变量，channel自身也是变量，可以传递自己。

引用参考信息：https://dave.cheney.net/2014/03/19/channel-axioms
