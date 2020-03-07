# 1.13 版本差异

### 继承了C语言关于数字字面量(number literal)的语法形式，和1978年发布的K&R C一样，Go仅支持十进制、八进制、十六进制和十进制形式的浮点数的数字字面量形式
```

a := 53         //十进制
 
b := 0700       // 八进制，以"0"开头
c := 0xaabbcc   // 十六进制 以"0x"开头
 
c1 := 0Xddeeff  // 十六进制 以"0X"开头
 
f1 := 10.24     // 十进制浮点数
f2 := 1.e+0     // 十进制浮点数
f3 := 31415.e-4 // 十进制浮点数
 
```

增加二进制数字字面量，以0b或0B标示

* 在保留以”0″开头的八进制数字字面量形式的同时，增加以”0o”或”0O”开头的八进制数字字面量形式
* 增加十六进制形式的浮点数字面量，以0x或0X开头的、形式如0×123.86p+2的浮点数
* 为提升可读性，在数字字面量中增加数字分隔符”_”，分隔符可以用来分隔数字(起到分组提高可读性作用，比如每3个数字一组)，也可以用来分隔前缀与第一个数字。

### Go 1.13中关于语言规范方面的另一个变动点是取消了移位操作(>>的<<)的右操作数仅能是无符号数的限制，以前必须的强制到uint的转换现在不必要了：

```
var i int = 5
 
fmt.Println(2 << uint(i)) // before go 1.13
fmt.Println(2 << i)       // in go 1.13 and later version

```

注意事项：
* go 1.12版本在go.mod文件中增加了一个go version的指示字段，用于指示该module内源码所使用的 go版本。
* Go 1.13的发布文档强调了只有在go.mod中的go version指示字段为go 1.13(以及以后版本)时，上述的语言特性变更才会生效
* 如果repo下没有go.mod或者单独在某个没有go.mod的目录下使用go 1.13编译器运行上面代码，则是无问题的。

### GOSUMDB
Go 1.13提供了GOSUMDB环境变量用于配置Go校验和数据库的服务地址（和公钥），其默认值为”sum.golang.org”，这也是Go官方提供的校验和数据库服务(大陆Gopher可以使用sum.golang.google.cn)。
出于安全考虑，建议保持GOSUMDB开启。但如果因为某些因素，无法访问GOSUMDB（甚至是sum.golang.google.cn），可以通过下面命令将其关闭：
```

go env -w GOSUMDB=off

```
GOSUMDB关闭后，仅能使用本地的go.sum进行包的校验和校验。

### 面向私有模块的GOPRIVATE
Go 1.13提供了GOPRIVATE变量，用于指示哪些仓库下的module是private，不需要通过GOPROXY下载，也不需要通过GOSUMDB去验证其校验和。不过要注意的是GONOPROXY和GONOSUMDB可以override GOPRIVATE中的设置，因此设置时要谨慎，比如下面的例子：
```
GOPRIVATE=pkg.tonyba.com/private
GONOPROXY=none
 
GONOSUMDB=none
```
GOPRIVATE指示pkg.tonybai.com/private下的包不经过代理下载，不经过SUMDB验证。但GONOPROXY和GONOSUMDB均为none，意味着所有module，不管是公共的还是私有的，都要经过proxy下载，经过sumdb验证。前面提到过了，GONOPROXY和GONOSUMDB会override GOPRIVATE的设置，因此在这样的配置下，所有依赖包都要经过proxy下载，也要经过sumdb验证。不过这个例子中的GOPRIVATE的值也不是一无是处，它可以给其他go tool提供私有module的指示信息。

### 性能

* defer性能提升。
* 优化后的逃逸分析(escape analysis)让编译器在选择究竟将变量分配在stack上还是heap上的时候更加精确。在老版本里分配到heap上的变量，在Go 1.13中可能就会分配到stack上，从而减少内存分配的次数，一定程度上减轻gc的压力，达到性能提升的目的。
* sync包中Mutex、RWMutex的方法的inline化带来的性能提升，官方说法是10%。

### 其他

* Go 1.13现在支持Android 10了；对MacOS的支持需要至少10.11版本；

* godoc独立：godoc不再和go、gofmt放入go release版中，需要godoc的，需要单独从golang.org/x/tools/cmd/godoc中下载安装；

* crypto/tls默认开启tls 1.3支持；

* unicode包支持的unicode标准从10.0版本升级到Unicode 11.0版本。

