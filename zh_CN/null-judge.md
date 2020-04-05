# null judge

### int空值是0，

### string空值是””而不是null或者nil（区别于其他语言），

### Slice空值是长度为0的Slice而不是nil，

### map空值是nil，

### error空值是nil，

### struct空值是一个“所有成员都是空值”的空Struct而不是nil，

### 不能单纯地判断一个struct是不是nil，因为它永远不可能是nil，可以通过返回一个error来判断是否为空，
### golang标准库里的常见做法：if err != nil

### 常见类型的 nil

```
bool      -> false                              
numbers -> 0                                 
string    -> ""      

pointers -> nil
slices -> nil
maps -> nil
channels -> nil
functions -> nil
interfaces -> nil
```