---
tags: 
aliases:
  - Inheritance
---

双亲委派模型要求除了顶层的启动类加载器外，其余的[[Java 类加载器]]都应有自己的父类加载 器。不过这里类加载器之间的父子关系一般不是以[[继承]]的关系来实现的，而是通常使用[[组合]]关系来复用父加载器的代码。

![](https://gd-hbimg.huaban.com/0af397ce0a099951366bc80b4afff7b5f39528d482f50-bz5pQp_fw1200webp)

![[Java 类加载器#启动类加载器]]

![[Java 类加载器#扩展类加载器]]

![[Java 类加载器#应用程序类加载器]]

## 工作过程

如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求(它的搜索范围中没有找到所需的类)时，子加载器才会尝试自己去完成加载。

例如类java.lang,Object,它存放在rt.jar之中,无论哪一个类加载器要加载这个类，最终都是委派给处于模型最顶端的启动类加载器进行加载，因此Object类在程序的各种类加载器环境中都能够保证是同一个类。反之，如果没有使用双亲委派模型，都由各个 类加载器自行去加载的话，如果用户自己也编写了一个名为java.lang.Object的类，并放在程序的 ClassPath中，那系统中就会出现多个不同的Object类，[[Java]]类型体系中最基础的行为也就无从保证，应用程序将会变得一片混乱。

```java
protected synchronized Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {  
    // 首先，检查请求的类是否已经被加载过了  
    Class c = findLoadedClass (name);  
    if (c == null) {  
        try {  
            if (parent != null) {  
                c = parent.loadClass(name, false);  
            } else {  
                c = findBootstrapClassOrNull(name);  
            }  
        } catch (ClassNotFoundException e) {  
            // 如果父类加载器抛出ClassNotFoundException  
            // 说明父类加载器无法完成加载请求        }  
        if (c == null) {  
            // 在父类加载器无法加载时  
            // 再调用本身的findClass方法来进行类加载            
            c = findClass(name);  
        }  
    }  
    if (resolve) {  
        resolveClass(c);  
    }  
    return c;   
}
```

>【注:】这就是为什么采用类加载方案的热修复需要冷启动生效的原因：补丁合成好之前类已加载，想要替换bug类，需要重新启动软件，重新加载修复好的类。


