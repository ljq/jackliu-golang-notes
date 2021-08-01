# sync 同步机制

* WaitGroup
* Mutex （基于Mutex的读写锁 sync.RWMutex）
* Cond


# Mutex（互斥锁）
Mutex 为互斥锁，Lock() 加锁，Unlock() 解锁
在一个 goroutine 获得 Mutex 后，其他 goroutine 只能等到这个 goroutine 释放该 Mutex
使用 Lock() 加锁后，不能再继续对其加锁，直到利用 Unlock() 解锁后才能再加锁
在 Lock() 之前使用 Unlock() 会导致 panic 异常
已经锁定的 Mutex 并不与特定的 goroutine 相关联，这样可以利用一个 goroutine 对其加锁，再利用其他 goroutine 对其解锁
在同一个 goroutine 中的 Mutex 解锁之前再次进行加锁，会导致死锁
适用于读写不确定，并且只有一个读或者写的场景


```
var mutex *sync.RWMutex
mutex = new(sync.RWMutex)
mutex.Lock()
//--code
mutex.Unlock()
```

# sync.WaitGroup的用法

和使用管道场景以及的区别：
WaitGroup 对象内部有一个计数器，最初从0开始，它有三个方法：Add(), Done(), Wait() 用来控制计数器的数量。Add(n) 把计数器设置为n ，Done() 每次把计数器-1 ，wait() 会阻塞代码的运行，直到计数器地值减为0。

**相对于使用管道来说，WaitGroup 更轻量级**

```
func main() {
    wg := sync.WaitGroup{}
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func(i int) {
            fmt.Println(i)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

# sync.WaitGroup注意事项
* 计数器不能为负值
* WaitGroup对象不是一个引用类型（**传参需要地址，否则会导致死锁**）
