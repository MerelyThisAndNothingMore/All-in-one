#coding 
# Introduction 
Modifiers in program  are used to convey additional stipulations.
[[Access Control Modifiers]]
[[Static modifier]]
## The abstract modifier 
A method of a class may be declared as abstract, in which case its signature is provided but without an implementation of the method body.
Abstract methods are an advanced feature of object-oriented programming to be combined with inheritance, In short, any subclass of a class with abstract methods is expected to provide a concrete implementation for each abstract method.
## The final modifier 
A variable that is declared with the final modifier can be initialized as part of that declaration, but can never again be assigned a new value. If it is a base type, then it is a constant. If a reference variable is final, then it will always refer to the same object (even if that object changes its internal state).
Designating a method or an entire class as final has a completely different consequence, only relevant in the context of inheritance. A final method cannot be overridden by a subclass, and a final class cannot even be subclassed.