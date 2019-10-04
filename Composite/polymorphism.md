# polymorphism 

# 多态特性
* 多态：同一件事情由于条件不同产生的结果不同；
* 由于Go语言中结构体不能相互转换，所以没有结构体类型的多态，只有基于接口的多态。   
  这符合Go语言对面向对象的诠释； 
   
# 多态和泛型的区别

* 泛型是当我们使用这个泛型类时候再去确定这个类里面的成员具体什么类型的，两者*本质*不是一个层次，多态不能实现泛型。
* 多态是在继承层面上，即根据实际运行时候来确定具体的实现。
* 泛型本质上并不是多态的一种特例，多态并不能实现泛型。
* 引入泛型的本质是为了安全，把编译时能发现的错误不带到runtime运行时。


# polymorphic characteristics
* Polymorphism: the same thing produces different results due to different conditions;
* Since the structures in the Go language cannot be converted to each other, there is no polymorphism of the structure type, only interface-based polymorphism.
   This is in line with the object-oriented interpretation of the Go language;
   
# Difference between polymorphism and generics

* Generics are when we use this generic class to determine what type of member is in the class, the two *the essence * is not a level, polymorphism can not achieve generic.
* Polymorphism is at the level of inheritance, that is, based on the actual operation time to determine the specific implementation.
* Generics are not a special case of polymorphism in nature, and polymorphism does not implement generics.
* The essence of introducing generics is to ensure that errors found at compile time are not brought to runtime runtime.

