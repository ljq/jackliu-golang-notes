

### Go tool pprof性能监控调试工具基本使用说明

#### Go tool pprof使用方式

go中有pprof包来做代码的性能监控主要涉及两个pkg：

```
#web服务器:
import (
    "net/http"
    _ "net/http/pprof"
)

#一般应用程序(实际应用无web交互)
import (
    "net/http"
    _ "runtime/pprof"
)
```

net/http/pprof中只是使用runtime/pprof包来进行封装了一下，并在http端口上暴露出来。


#### Go tool pprof辅助工具安装(图形工具graphviz为例)

*   __Windows__：  
1.官方下载安装包: http://www.graphviz.org/download/  
下载Stable稳定版本(.msi)  
2.配置PATH系统环境变量：  
C:\Program Files (x86)\Graphviz2.38\bin  

*   __Linux(例：Centos)__

方式1).添加repo依赖http://204.178.9.49/graphviz-rhel.repo
```   
yum list available 'graphviz*'
yum install 'graphviz*'  --skip-broken
#备注：--skip-broken可选：跳过错误依赖，不加这个参数会提示安装包依赖错误，因为这里并不需要其它的安装包，所以跳过即可。
```
  --skip-broken可选：跳过错误依赖，不加这个参数会提示安装包依赖错误，因为这里并不需要其它的安装包，所以跳过即可。

方式2).源码包编译安装
./configure
make
make install

*   __MacOS__:  
brew install graphviz


#### Go tool pprof常用基本调试基本命令(默认30s采集时间，可通过--seconds)


###### HTTP场景(参数可选:--text)：

Heap profile:  
```
go tool pprof --text http://localhost:8080/debug/pprof/heap  
```
CPU profile:  
```
go tool pprof --text http://localhost:8080/debug/pprof/profile  
```
Goroutine blocking profile:  
```
go tool pprof --text http://localhost:8080/debug/pprof/block  
```

1.实时通过地址查看浏览器: http://localhost:8080/debug/pprof/;
2.通过生成的profile文件分析;  
选择指定的profile压缩gz文件(.gz),使用go tool pprof进入
```
go tool pprof http://localhost:8080/debug/pprof/profile
#结束后直接进入交互：
(pprof)
  web
(pprof)
```

如查看历史调试文件信息，通过指定的profile文件进入即可:
go tool pprof [*.gz]

pprof交互基本命令：web 直接生成web浏览器可访问的svg图;  
(其他命令自行摸索)
Windows下自动生成.svg文件且调用默认浏览器访问;    
MacOS下自动生成.gz文件，系统限制可根据提示文件路径通过手动访问查看;  

【注意事项】：
profile文件为空的问题, heap和block一般不受影响。
执行交互web命令会报:
```
(pprof) web
profile is empty
(pprof) 
```
产生原因：
pprof内存分析器采取抽样的方式，它仅仅从一些内存分配的子集中收集信息。有可能对一个对象的采样与被采样对象的大小成比例。通过使用go test --memprofilerate标识，或者通过程序启动时 的运行配置中的MemProfileRate变量来改变调整这个采样的比例。如果比例为1，则会导致全部申请的信息都会被收集，但是这样的话将会使得执行变慢。默认的采样比例是每512KB的内存申请就采样一次。

*   方法1).在进行调试时，指定运行参数，或运行代码中动态调整参数
```
go tool pprof --text http://localhost:8080/debug/pprof/profile
```
此命令将会打印耗费最多CPU时间的函数列表。 
这里有几种可用的输出形式，最实用的有 --text, --web 和 --list。运行 "go tool pprof" 来得到完整的列表。 

【备注】：
实际测试时，MacOS下基本是空的,需要指定参数。

*   方法2).设置环境变量(__此方法极不推荐!__)设置Go环境变量 GODEBUG="memprofilerate=1".

*   __通过控制采样的比例和行为，可以达到性能调试粒度的控制！__ 



#### 附pprof 交互命令help说(部分命令需要第三方工具支持)：
```
(pprof) help
 
 Commands:
   cmd [n] [--cum] [focus_regex]* [-ignore_regex]*
       Produce a text report with the top n entries.
       Include samples matching focus_regex, and exclude ignore_regex.
       Add --cum to sort using cumulative data.
       Available commands:
         callgrind    Outputs a graph in callgrind format
         disasm       Output annotated assembly for functions matching regexp or address
         dot          Outputs a graph in DOT format
         eog          Visualize graph through eog
         evince       Visualize graph through evince
         gif          Outputs a graph image in GIF format
         gv           Visualize graph through gv
         list         Output annotated source for functions matching regexp
         pdf          Outputs a graph in PDF format
         peek         Output callers/callees of functions matching regexp
         png          Outputs a graph image in PNG format
         proto        Outputs the profile in compressed protobuf format
         ps           Outputs a graph in PS format
         raw          Outputs a text representation of the raw profile
         svg          Outputs a graph in SVG format
         tags         Outputs all tags in the profile
         text         Outputs top entries in text form
         top          Outputs top entries in text form
         tree         Outputs a text rendering of call graph
         web          Visualize graph through web browser
         weblist      Output annotated source in HTML for functions matching regexp or address
   peek func_regex
       Display callers and callees of functions matching func_regex.
 
   dot [n] [focus_regex]* [-ignore_regex]* [>file]
       Produce an annotated callgraph with the top n entries.
       Include samples matching focus_regex, and exclude ignore_regex.
       For other outputs, replace dot with:
       - Graphic formats: dot, svg, pdf, ps, gif, png (use > to name output file)
       - Graph viewer:    gv, web, evince, eog
 
   callgrind [n] [focus_regex]* [-ignore_regex]* [>file]
       Produce a file in callgrind-compatible format.
       Include samples matching focus_regex, and exclude ignore_regex.
 
   weblist func_regex [-ignore_regex]*
       Show annotated source with interspersed assembly in a web browser.
 
   list func_regex [-ignore_regex]*
       Print source for routines matching func_regex, and exclude ignore_regex.
 
   disasm func_regex [-ignore_regex]*
       Disassemble routines matching func_regex, and exclude ignore_regex.
 
   tags tag_regex [-ignore_regex]*
       List tags with key:value matching tag_regex and exclude ignore_regex.
 
   quit/exit/^D
         Exit pprof.
 
   option=value
       The following options can be set individually:
           cum/flat:           Sort entries based on cumulative or flat data
           call_tree:          Build context-sensitive call trees
           nodecount:          Max number of entries to display
           nodefraction:       Min frequency ratio of nodes to display
           edgefraction:       Min frequency ratio of edges to display
           focus/ignore:       Regexp to include/exclude samples by name/file
           tagfocus/tagignore: Regexp or value range to filter samples by tag
                               eg "1mb", "1mb:2mb", ":64kb"
 
           functions:          Level of aggregation for sample data
           files:
           lines:
           addresses:
 
           unit:               Measurement unit to use on reports
 
           Sample value selection by index:
            sample_index:      Index of sample value to display
            mean:              Average sample value over first value
 
           Sample value selection by name:
            alloc_space        for heap profiles
            alloc_objects
            inuse_space
            inuse_objects
 
            total_delay        for contention profiles
            mean_delay
            contentions
 
   :   Clear focus/ignore/hide/tagfocus/tagignore
(pprof)

```