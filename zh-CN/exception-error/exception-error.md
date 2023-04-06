# Excepton or Error


### Golang处理异常的设计思想

* 如果函数可能出现异常则应该把异常作为返回值，没有异常就返回 nil；
* **卫述语句**：当可能出现异常的函数时，都应该主动进行if语句检查，并做出反应；


### 自定义错误或异常

* 创建信息： ```errors.New("message")```，更建议使用```fmt.Errorf("%v", "message")```格式化输出
* 打印异常： ```err.Error()```


