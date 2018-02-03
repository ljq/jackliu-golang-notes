##### json.Unmarshal()    
注意：如果JSON中的字段在Go目标类型中不存在，json.Unmarshal() 函数在解码过程中会丢弃该字段。   
未知类型,遵循规则：   
1.JSON中的布尔值将会转换为Go中的bool类型  
2.数值会被转换为Go中的float64类型   
3.字符串转换后还是string类型   
4.JSON数组会转换为[]interface{} 类型   
5.JSON对象会转换为map[string]interface{}类型   
6.null值会转换为nil   
