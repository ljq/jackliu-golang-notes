# reflect 反射

### reflect 包类型定义

```
//reflect 包类型定义

type Kind Sign

const (
    Invalid Kind = iota  // 非法类型
    Bool                 // 布尔型
    Int                  // 有符号整型
    Int8                 // 有符号8位整型
    Int16                // 有符号16位整型
    Int32                // 有符号32位整型
    Int64                // 有符号64位整型
    Uint                 // 无符号整型
    Uint8                // 无符号8位整型
    Uint16               // 无符号16位整型
    Uint32               // 无符号32位整型
    Uint64               // 无符号64位整型
    Uintptr              // 指针
    Float32              // 单精度浮点数
    Float64              // 双精度浮点数
    Complex64            // 64位复数类型
    Complex128           // 128位复数类型
    Array                // 数组
    Chan                 // 通道
    Func                 // 函数
    Interface            // 接口
    Map                  // 映射
    Ptr                  // 指针
    Slice                // 切片
    String               // 字符串
    Struct               // 结构体
    UnsafePointer        // 底层指针
)

```

### 反射性能的一般原则

基准测试结果的数值分析的一般原则建议：
* 能使用原生代码时，尽量避免反射操作。
* 提前缓冲反射值对象，对性能有很大的帮助。
* 避免反射函数调用，实在需要调用时，先提前缓冲函数参数列表，并且尽量少地使用返回值。

