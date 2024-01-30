---
tags: 
alias:
---
在[[Java]]代码中声明native方法后，需要在native文件中实现对应方法并进行声明。
### build.gradle
```groovy
externalNativeBuild {  
    cmake {  
        path file('src/main/cpp/CMakeLists.txt')  
        version '3.22.1'  
    }  
}
```
