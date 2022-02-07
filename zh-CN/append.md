# append 注意事项

### append: append发生扩容的动作后的容量增长问题


##### 扩容预估的处理流程分两种情况：
* append单个元素，或者append少量的多个元素，这里的少量指double之后的**容量能容纳**，这样就会走以下扩容流程，不足1024，双倍扩容，超过1024的，1.25倍扩容。
* 若是append多个元素，且double后的**容量不能容纳**，直接使用预估的容量。

注意：
* 根据double后的容量能不能容纳，再决定预估容量。
* 但是此非最后实际容量，还要结合slice的类型size算出新的容量所需的内存情况capmem，然后再进行capmem向上取整，得到新的所需内存，除上类型size，得到真正的最终容量,作为新的slice的容量。  
**所以一般说容量翻倍或者超过1024增长1.25倍是不严谨的！**
* 出于内存的高效利用考虑，还要进行内存对齐!
```capmem := roundupsize(uintptr(newcap) * uintptr(et.size))```
newcap就是前文中计算出的newcap，et.size代表slice中一个元素的大小，capmem计算出来的就是此次扩容需要申请的内存大小。**roundupsize**函数就是处理内存对齐的函数。

###### 附：相关处理逻辑
```
func roundupsize(size uintptr) uintptr {
 if size < _MaxSmallSize {
  if size <= 1024-8 {
   return uintptr(class_to_size[size_to_class8[(size+7)>>3]])
  } else {
   return uintptr(class_to_size[size_to_class128[(size-1024+127)>>7]])
  }
 }
 if size+_PageSize < size {
  return size
 }
 return round(size, _PageSize)
```