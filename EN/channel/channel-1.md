# channel Usage scenarios


Channel usage scenarios (data flow)

* Message passing, message filtering
* Signal broadcasting
* Event subscription and broadcasting
* Request and response forwarding
* Task distribution
* Summary of results
* Concurrency control
* Synchronous and asynchronous
```

Basic operation and precautions of channel
There are 3 states of channel:

* nil, uninitialized state, only declared, or manually assigned to nil
* active, normal channel, readable or writable
* closed, closed, ** Do not mistakenly believe that after closing the channel, the value of the channel is nil **
* The channel can perform 3 operations: Read, Write, Close

| Operation | nil channel | normal channel | closed channel
|: --------: |: -------: |: ------: |: ----------------: |
| <-ch blocking | success or blocking | zero value read |
| ch <-blocking | success or blocking | panic |
| close (ch) panic | Success | panic |

** 3 operations X 3 channel states = 9 scenarios combined **


* 1. Use for range to read channel

Scenes:

When you need to continuously read data from the channel.

Use for-range to read the channel, which is safe and convenient. When the channel is closed, the for loop will automatically exit. There is no need to actively monitor whether the channel is closed. The zero value of the data type.

usage
```
for x: = range ch {
    fmt.Println (x)
}
```

* 2. Use v, ok: = <-ch + select to determine whether the channel is closed
Scenes
v, ok: = <-ch + select operation to determine whether the channel is closed

The results and meaning of ok:
-`true`: read the channel data, not sure if it is closed, maybe the channel has saved data, but the channel is closed
-`false`: The channel is closed and no data is read.

* 3. Use select to handle multiple channels
Scenes
When multiple channels need to be processed simultaneously, but only the channel that occurs first is processed

Select can monitor the situation of multiple channels at the same time, and only handle unblocked cases. When the channel is nil, the corresponding case is always blocked, regardless of reading or writing. Special attention: Under normal circumstances, writing to the nil channel is panic.


* 4. Use the channel statement to control read and write permissions
Scenes
When the coroutine reads or writes to a channel

purpose:

Make the code more readable and easier to maintain,
Prevent the read-only coroutine from writing data to the channel, but the channel is closed, causing panic.

usage
If the coroutine only writes to a channel, the channel is declared as write-only.
If the coroutine only has a read operation on a channel, the channe is declared read-only.

```
// Only the generator writes outCh and returns the statement
// <-chan int, can prevent other coroutines from using this channel indiscriminately, causing hidden bugs
func generator (int n) <-chan int {
    outCh: = make (chan int)
    go func () {
        for i: = 0; i <n; i ++ {
            outCh <-i
        }
    } ()
    return outCh
}

// consumer read-only inCh data, declared as <-chan int
// Can prevent it from writing data to inCh
func consumer (inCh <-chan int) {
    for x: = range inCh {
        fmt.Println (x)
    }
}
```

* 5. Use buffered channels to enhance concurrency
Scenes
asynchronous

There are buffer channels for multiple coroutines to process at the same time, which can improve concurrency to a certain extent.
usage
```
// no buffer
ch1: = make (chan int)
ch2: = make (chan int, 0)
// buffered
ch3: = make (chan int, 1)
// Use 5 `do` coroutines to process input data simultaneously
func test () {
    inCh: = generator (100)
    outCh: = make (chan int, 10)

    for i: = 0; i <5; i ++ {
        go do (inCh, outCh)
    }

    for r: = range outCh {
        fmt.Println (r)
    }
}

func do (inCh <-chan int, outCh chan <-int) {
    for v: = range inCh {
        outCh <-v * v
    }
}
```

* 6. Add timeout to operation
Scenes
Operations requiring timeout control

Use select and time.After, to see which operation and timer return first, the processing is completed first, the effect of timeout control is achieved

* 7. Use time to achieve channel non-blocking read and write
Scenes
I don't want to waste time on channel reading and writing

It is an extension to add a timeout to the operation. The operation here is the reading or writing of the channel
Note: time.After waiting can be replaced with default, it is the effect of returning immediately when the channel is blocked

* 8. Use close (ch) to close all downstream coroutines
Scenes
When exiting, display to notify all coroutines to exit

All coroutines that read ch will receive the close (ch) signal

* 9. Use chan struct {} as signal channel
Scenes
Use channels to pass signals instead of data

When there is no data to pass, pass an empty struct

* 10. Use channel to pass pointer of structure instead of structure
Scenes
When using channel to transfer structure data

** The channel essentially transfers a copy of the data. The smaller the copied data, the higher the transmission efficiency. The transfer structure pointer is more efficient than the transfer structure

* 11. Use channel to pass channel
Scenes
There are a lot of usage scenarios, usually used to get results.

The channel can be used to pass variables, the channel itself is a variable, you can pass yourself.

Reference information: https://dave.cheney.net/2014/03/19/channel-axioms