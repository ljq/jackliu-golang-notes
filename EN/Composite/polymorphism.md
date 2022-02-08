# polymorphism 

# polymorphic characteristics
* Polymorphism: the same thing produces different results due to different conditions;
* Since the structures in the Go language cannot be converted to each other, there is no polymorphism of the structure type, only interface-based polymorphism.
   This is in line with the object-oriented interpretation of the Go language;
   
# Difference between polymorphism and generics

* Generics are when we use this generic class to determine what type of member is in the class,**the two the essence is not a level**, polymorphism can not achieve generic.
* Polymorphism is at the level of inheritance, that is, based on the actual operation time to determine the specific implementation.
* Generics are not a special case of polymorphism in nature, and polymorphism does not implement generics.
* The essence of introducing generics is to ensure that errors found at compile time are not brought to runtime runtime.

