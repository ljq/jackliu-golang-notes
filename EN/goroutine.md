# goroutine

### Golang does not use OS layer process but USES language level (Goroutine) to deal with concurrency & parallel Task Task scheme (for business) personal thinking and practical thinking -Jack Liu qiu

* 1. Process management using OS level, although the low cost and quick, but the CPU performance overhead is large, Golang through the introduction of Goroutine, construct a SandBox SandBox container type, internal concurrent processing tasks in language level, can be "isolation" in the system level, construct a Goroutine pool at the same time, also can guarantee under the premise of reliable performance, security is enhanced.  
Overall direction of the large-scale distributed system at present, most are based on OS layer (rather than a system level) in order to achieve a controlled standard control, level the OS kernel CPU overhead to a minimum to ensure that the OS layer smooth running, which also makes represented by the Linux server OS more "focus" do Base underlying fundamental business support.

* 2. At the system level, apart from the programming language level, the server is more and more servitization, distribution and clustering, so as to deal with more and more complex businesses. The existing programming language historical legacy and ecological environment and other factors processing cost is high, for multi-core CPU utilization is not high, the performance problem is prominent.
Golang's advantage on the server side is that its very low cost frees developers from the single-process-like programming languages of the past, providing the lowest cost of a quick transition to a parallel programming mindset.
Goroutine concurrent execution mode, not placed in the system process processing, benefits:
(1) security isolation design, which limits the boundaries of Task Process processing in the Process, and USES relatively uniform standard processing methods in large-scale clustered servers to minimize problems caused by OS differences;
(2) reduce the cost of OS process, without dragging down system CPU resources due to golang execution;
(3) the behavior and state of concurrent tasks are controllable, with small memory footprint and free capacity expansion;
(4) high reliability of lightweight in coroutine processing;
(5). With this mechanism, applications and programs for large and scalable computing and batch Task tasks can be built at a lower cost. Since Golang was originally positioned as a system-level programming language, binary performance will not be poor.    

The current norm for large systems in the industry for business processing should include at least two things:
1. Support the minimum cost organization of large-scale data and calculation processing;
2. The behavior and status of Task tasks can be reliable and controllable at low cost.

Cloud computing pursues the lowest cost configuration and the best resource scheduling of hardware and software resources under the premise of security and control of the server. The important reason for all the distributed, free scaling and clustering is also for this reason, and Goroutines has great potential in this respect.  

The official sentence is relatively straightforward and representative:
Do not communicate by sharing memory; Instead, share memory by shares.
Instead of communicating through Shared memory, communicate through Shared memory.  

Define the two concepts of concurrency and parallelism:
Concurrency is not parallel: concurrency is run "simultaneously" by switching time, parallel is multi-core multi-threading
Goroutine communicates with Shared memory, not Shared memory. So you can more make full use of multi-core CPU and memory resources at the same time, and is relatively reliable, "coroutines" is similar to "pipe" kind of mode of thinking, here, communication is more important, these Golang has done the underlying implementation, easy for developers to compare some, most of the attention to manage these valves inward and outward.
The Linux pipeline is a very good design.  

This should not to say that the former scheme or other language processing idea is wrong, not beautiful or biased, many are often limited by hardware operation performance in the history of the comprehensive factors, was weighed down the best solution, such as the hardware cost is too high, now with the rapid development of hardware and software and low cost has the condition to do this. After all, there are various factors based on the current situation.
No matter how fast the performance is, reliability must be put on the top of the list. If a program is relatively unreliable, no matter how fast the program runs, it will be meaningless. I think this is an important consideration for Golang from the perspective of software engineering.   

The Golang design philosophy should be the same as Unix: simplicity is the best category to think about in the development of large-scale engineering systems.

The concurrent & parallel programming concept is the norm of future programming thinking and the law of things development for developers.  

At least in the development of company products and projects, Golang is at least the major language of the future, because with the continuous growth of data, it is necessary to select the most suitable technology in terms of performance, reliability and development. Golang is very consistent with this. Golang is not just a simple programming language.  
The reason for the enduring popularity of the C development language is the pursuit of solving practical problems in the simplest way. Golang is the major development language in the future.  
### Personal practice ideas:
![image](https://github.com/iotd/jackliu-golang-notes/blob/master/EN/done-mode.jpg)

------------------ Liu qiu Date: 2017-05-30