##### json.Marshal()  
默认转换规则：  
1.布尔型转换为 JSON 后仍是布尔型　， 如true -> true  
2.浮点型和整数型转换后为JSON里面的常规数字，如 1.23 -> 1.23  
3.字符串将以UTF-8编码转化输出为Unicode字符集的字符串，特殊字符比如<将会被转义为\u003c  
4.数组和切片被转换为JSON 里面的数组，[]byte类会被转换为base64编码后的字符串，slice的零值被转换为null  
5.结构体会转化为JSON对象，并且只有结构体里边以大写字母开头的可被导出的字段才会被转化输出，而这些可导出的字段会作为JSON对象的字符串索引  
6.转化一个map 类型的数据结构时，该数据的类型必须是 map[string]T（T 可以是encoding/json 包支持的任意数据类型）  


___


##### json.Unmarshal()    
注意：如果JSON中的字段在Go目标类型中不存在，json.Unmarshal() 函数在解码过程中会丢弃该字段。   
未知类型,遵循规则：   
1.JSON中的布尔值将会转换为Go中的bool类型  
2.数值会被转换为Go中的float64类型   
3.字符串转换后还是string类型   
4.JSON数组会转换为[]interface{} 类型   
5.JSON对象会转换为map[string]interface{}类型   
6.null值会转换为nil   
