# goroutine

Channel 实际上是个环形队列。队列空间就在这个channel结构体之后申请空间。
dataqsiz -> data queue size 队列大小。elemsize 元素的大小。
Lock用来保证线程(协程)安全。recvq和sendq分别用来保存对应的阻塞队列。

```
struct    Hchan
{
    uintgo    qcount;            // 队列q中的总数据数量
    uintgo    dataqsiz;        // 环形队列q的数据大小
    uint16    elemsize;
    bool    closed;
    uint8    elemalign;
    Alg*    elemalg;        // interface for element type
    uintgo    sendx;            // 发送index
    uintgo    recvx;            // 接收index
    WaitQ    recvq;            // 因recv而阻塞的等待队列
    WaitQ    sendq;            // 因send而阻塞的等待队列
    Lock;
}
```

### 阻塞注意事项
死锁的一般情况：
* 流出无流入；
* 流入无流出

注意事项：不会阻塞的特殊情况

```
func main() {
    ch := make(chan int)
    go func() {
       ch <- 1
    }()
}
```
原因：main函数先于goroutine执行完毕，所以无数据流入主goroutine，不会发送阻塞报错。



### goroutine 用法






