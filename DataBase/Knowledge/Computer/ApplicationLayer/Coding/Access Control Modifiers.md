#softwareEngineering 
# Introduction 
Access control [[Modifiers]] control the level of access. The ability to limit access among [[class]]es supports a key principle of [[Object-oriented programming]] known as [[Encapsulation]].
# Implementation 
## Java
| [[Modifiers]] |                                                                        remark                                                                         |
|:-------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------:|
|    public     |            all classes may access the defined aspect.  <br> each public [[class]] must be defined in a separate file named classname.java             |
|   protected   | Classes that are designated as subclasses of the given [[class]] through inheritance.<br> Classes that belong to the same package as the given class. |
|    private    |                                                               code within the [[class]]                                                               |
|    default    |                                             package-private: classes in the same package to have access.                                              |
