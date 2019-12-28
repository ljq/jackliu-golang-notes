# package time

### time Switching considerations

The Go language specifies the time origin

```
package main

import (
	"time"
	"fmt"
)

func main()  {
	//Current time format, memory rule: year 2016 | - | - | - 3 (15:00, 3pm) | - 4 - | - 5
	nowTime := time.Now().Format("2006-01-02 15:04:05")
	fmt.Println(nowTime)
}
```