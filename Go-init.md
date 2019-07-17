### init()函数特性

* init()在main包执行之前
* init()自动执行,不能显示调用
* 同一个Go文件中可定义多个init()函数，顺序执行
* 同一个package中不同文件，将文件名按字符串进行字母和数字自然排序，之后顺序调用各文件中的init()
* import导入：
    * 标示符 **_** 操作，本质是引入package且不直接使用内部函数，仅仅调用init()初始化
    * 按照import导入顺序调用包中init()
    * package的init()在被引用时自动被调用
    * package存在依赖，调用顺序为最后被依赖的最先被执行初始化
    * package被其他多个包import，只能被初始化一次
