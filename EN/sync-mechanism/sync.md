# sync mechanism

* WaitGroup
* Mutex（Read/write lock sync.rwmutex based on Mutex）
* Cond


# Sync.WRMutex handles common read and write thread safety issues

* RWMutex (read-write lock)
RWMutex is a single write multiple read lock that can be added to multiple read locks or one write lock
Read locks block writes when occupied, not reads, and multiple goroutines can acquire read locks at the same time
A write lock prevents other goroutines (read and write) from entering, and the lock is exclusive to that goroutine
It is suitable for situations of reading more and writing less
* The Lock() and Unlock()
Lock() with a write Lock, Unlock() to Unlock the write Lock
If there are other read and write locks before a write Lock is added, the Lock() blocks until the Lock is available, and to ensure that the Lock is available, the blocked Lock() call excludes the new reader from the acquired Lock, that is, the write Lock has more privilege than the read Lock, and the write Lock takes precedence when there is a write Lock
Using Unlock() before Lock() causes panic exceptions
* RLock() and RUnlock()
RLock() reads the lock, RUnlock() reads the lock
When RLock() adds a read lock, if there is a write lock, the read lock cannot be added. Read locks can be added when there are only read locks or no locks, and read locks can be loaded multiple times
RUnlock() unlocks the lock, RUnlock() undoes the call to the word RLock(), and has no effect on other simultaneous read locks
Calling RUnlock() without a read lock causes a panic error
The number of RUnlock() must not be more than RLock(), otherwise panic will result

```
var mutex *sync.RWMutex
mutex = new(sync.RWMutex)
mutex.Lock()
//--code
mutex.Unlock()
```

# sync.WaitGroup usage

和使用管道场景以及的区别：
WaitGroup 对象内部有一个计数器，最初从0开始，它有三个方法：Add(), Done(), Wait() 用来控制计数器的数量。Add(n) 把计数器设置为n ，Done() 每次把计数器-1 ，wait() 会阻塞代码的运行，直到计数器地值减为0。

**相对于使用管道来说，WaitGroup 更轻量级**
And the difference between using pipeline scenarios and:
Inside the WaitGroup object is a counter that initially starts at 0 and has three methods: Add(), Done(), and Wait() to control the number of counters. Add(n) sets the counter to n, Done() sets the counter to -1 each time, and wait() blocks the code until the counter is reduced to 0.

**WaitGroup is more lightweight than using pipes**


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

# sync.WaitGroup considerations
* the counter cannot be negative
* the WaitGroup object is not a reference type (**pass-through requires an address, otherwise it will cause a deadlock**)

