### Go tool pprof performance monitoring and debugging tools basic instructions

#### Go tool pprof usage

There is a pprof package in go for performance monitoring of the code, which mainly involves two pkg:

```
#webserver:
import (
    "net / http"
    _ "net / http / pprof"
)

#General application (actual application without web interaction)
import (
    "net / http"
    _ "runtime / pprof"
)
```

Net / http / pprof only uses the runtime / pprof package to encapsulate it and expose it on the http port.


#### Go tool pprof auxiliary tool installation (graphviz as an example)

* __Windows__:
1.Official download and installation package: http://www.graphviz.org/download/
Download Stable stable version (.msi)
2. Configure the PATH system environment variable:
C: \ Program Files (x86) \ Graphviz2.38 \ bin

* __Linux (e.g. Centos) __

Way 1). Add repo dependency http://204.178.9.49/graphviz-rhel.repo
```
yum list available 'graphviz *'
yum install 'graphviz *' --skip-broken
#Note: --skip-broken is optional: skip error dependencies. If you do not add this parameter, the installation package dependency error will be prompted. Because no other installation packages are needed here, skip it.
```
  --skip-broken Optional: Skip wrong dependencies. If you don't add this parameter, the installation package dependency error will be prompted. Because no other installation packages are needed here, skip it.

Method 2). Compile and install the source package
./configure
make
make install

* __MacOS__:
brew install graphviz


#### Go tool pprof commonly used basic debugging basic commands (default 30s acquisition time, you can use --seconds)


####### HTTP scenario (optional: --text):

Heap profile:
```
go tool pprof --text http: // localhost: 8080 / debug / pprof / heap
```
CPU profile:
```
go tool pprof --text http: // localhost: 8080 / debug / pprof / profile
```
Goroutine blocking profile:
```
go tool pprof --text http: // localhost: 8080 / debug / pprof / block
```

1.View the browser through the address in real time: http: // localhost: 8080 / debug / pprof /;
2.Analyze through the generated profile file;
Select the specified profile compressed gz file (.gz) and use go tool pprof
```
go tool pprof http: // localhost: 8080 / debug / pprof / profile
#Enter the interaction directly after the end:
(pprof)
  web
(pprof)
```

If you want to view the historical debugging file information, you can enter it through the specified profile file:
go tool pprof [* .gz]

pprof interactive basic command: web directly generates svg map accessible by web browser;
(Other commands explore by themselves)
Automatically generate .svg files under Windows and call the default browser to access them;
Automatically generate .gz files under MacOS, system restrictions can be viewed manually by following the prompt file path

【Precautions】:
The profile file is empty. The heap and block are generally not affected.
Executing interactive web commands will report:
```
(pprof) web
profile is empty
(pprof)
```
cause:
The pprof memory analyzer takes a sampling approach, which only collects information from a subset of some memory allocations. It is possible to sample an object proportional to the size of the sampled object. Change the ratio of this sampling by using the go test --memprofilerate flag, or by using the MemProfileRate variable in the running configuration when the program starts. If the ratio is 1, all application information will be collected, but this will slow down execution. The default sampling ratio is sampled every 512KB of memory application.

* Method 1). When debugging, specify the running parameters, or dynamically adjust the parameters in the running code
```
go tool pprof --text http: // localhost: 8080 / debug / pprof / profile
```
This command will print a list of functions that consume the most CPU time.
There are several forms of output available. The most useful are --text, --web and --list. Run "go tool pprof" to get the complete list.

[Remarks]:
In actual testing, MacOS is basically empty, and you need to specify parameters.

* Method 2). Set the environment variable (__This method is highly recommended! __) Set the Go environment variable GODEBUG = "memprofilerate = 1".

* __ By controlling the proportion and behavior of sampling, you can achieve granular control of performance debugging! __



#### Attached to the pprof interactive command help said (some commands require the support of third-party tools):
```
(pprof) help
 
 Commands:
   cmd [n] [--cum] [focus_regex] * [-ignore_regex] *
       Produce a text report with the top n entries.
       Include samples matching focus_regex, and exclude ignore_regex.
       Add --cum to sort using cumulative data.
       Available commands:
         callgrind Outputs a graph in callgrind format
         disasm Output annotated assembly for functions matching regexp or address
         dot Outputs a graph in DOT format
         eog Visualize graph through eog
         evince Visualize graph through evince
         gif Outputs a graph image in GIF format
         gv Visualize graph through gv
         list Output annotated source for functions matching regexp
         pdf Outputs a graph in PDF format
         peek Output callers / callees of functions matching regexp
         png Outputs a graph image in PNG format
         proto Outputs the profile in compressed protobuf format
         ps Outputs a graph in PS format
         raw Outputs a text representation of the raw profile
         svg Outputs a graph in SVG format
         tags Outputs all tags in the profile
         text Outputs top entries in text form
         top Outputs top entries in text form
         tree Outputs a text rendering of call graph
         web Visualize graph through web browser
         weblist Output annotated source in HTML for functions matching regexp or address
   peek func_regex
       Display callers and callees of functions matching func_regex.
 
   dot [n] [focus_regex] * [-ignore_regex] * [> file]
       Produce an annotated callgraph with the top n entries.
       Include samples matching focus_regex, and exclude ignore_regex.
       For other outputs, replace dot with:
       -Graphic formats: dot, svg, pdf, ps, gif, png (use> to name output file)
       -Graph viewer: gv, web, evince, eog
 
   callgrind [n] [focus_regex] * [-ignore_regex] * [> file]
       Produce a file in callgrind-compatible format.
       Include samples matching focus_regex, and exclude ignore_regex.
 
   weblist func_regex [-ignore_regex] *
       Show annotated source with interspersed assembly in a web browser.
 
   list func_regex [-ignore_regex] *
       Print source for routines matching func_regex, and exclude ignore_regex.
 
   disasm func_regex [-ignore_regex] *
       Disassemble routines matching func_regex, and exclude ignore_regex.
 
   tags tag_regex [-ignore_regex] *
       List tags with key: value matching tag_regex and exclude ignore_regex.
 
   quit / exit / ^ D
         Exit pprof