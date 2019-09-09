# string


### 常用拼接方法

* += 来拼接
* bytes.Buffer
* strings.Builder ( >= go1.10 )

### 拼接方法性能对比

* 使用 += 的方法性能是最慢的，性能和其他两种差了好几个数量级;
* Buffer和Builder性能相差无几，Builder在内存的使用上要略优于Buffer;
* 官方推荐strings.Builder，优点: 性能好，代码清晰;
