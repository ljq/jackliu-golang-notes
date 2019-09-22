# package time

### time 转换注意事项

Go语言指定时间原点

```
package main

import (
	"time"
	"fmt"
)

func main()  {
	//当前时间格式化，记忆规律：创始年份2016 | 一 | 二 | 三（15时，即 3 pm） | 四 | 五
	nowTime := time.Now().Format("2006-01-02 15:04:05")
	fmt.Println(nowTime)
}
```