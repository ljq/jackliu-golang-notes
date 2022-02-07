# append Notes

### append: capacity growth problem after append capacity expansion occurs


##### The processing flow of expansion estimation is divided into two cases:
* Append a single element, or append a small amount of multiple elements, the small amount here refers to the **capacity after the double can accommodate**, so the following expansion process will be followed. If it is less than 1024, double the expansion, if it exceeds 1024, the expansion will be 1.25 times. .
* If append multiple elements, and the **capacity after double cannot accommodate**, use the estimated capacity directly.

Notice:
* According to whether the capacity after double can be accommodated, the estimated capacity is determined.
* However, this is not the final actual capacity, and the memory situation capmem required for the new capacity needs to be calculated in combination with the type size of the slice, and then the capmem is rounded up to obtain the new required memory. Divide the type size to get the real final capacity, as the capacity of the new slice.  
**So it is generally not rigorous to say that the capacity doubles or increases by 1.25 times over 1024!**
* For efficient use of memory, memory alignment is also required!
```capmem := roundupsize(uintptr(newcap) * uintptr(et.size))```
newcap is the newcap calculated in the previous section, et.size represents the size of an element in the slice, and what capmem calculates is the memory size that needs to be applied for this expansion. The **roundupsize** function is the function that handles memory alignment.

###### Attachment: Relevant processing logic
````
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
````