---
tags: 
aliases:
  - Parent Delegation Model
---

# 定义

双亲委派模型（Parent Delegation Model）是一种类加载机制，最早在 [[Java]] 的[[Java 类加载器|类加载器]]中引入。该模型通过将类加载的责任委派给父加载器，确保类加载的层次结构，从而避免类的重复加载和类加载器之间的冲突。双亲委派模型的核心思想是“自己要加载的[[类]]委托给父加载器加载，只有在父加载器无法加载该类时，自己才尝试加载该类。”

## 特点

1. **分层加载**：通过层次结构逐级委托类加载，确保核心类库先加载。
2. **避免重复加载**：确保每个类只被加载一次，避免类的重复加载。
3. **安全性**：防止核心类库被篡改和替换，确保系统安全。
4. **灵活性**：支持自定义类加载器，实现特殊需求的类加载。

# 工作原理

双亲委派模型的工作流程如下：

1. **类加载请求**：当类加载器需要加载一个类时，首先将该请求委托给父类加载器。
2. **逐级委托**：父类加载器再将请求逐级向上委托，直到委托到顶层的启动类加载器（Bootstrap ClassLoader）。
3. **类加载**：启动类加载器尝试加载该类，如果找到则加载成功并返回。如果找不到，则返回加载失败。
4. **向下传递**：如果启动类加载器无法加载该类，返回加载失败，委托请求逐级向下传递给子类加载器。
5. **自我加载**：最终由最初的类加载器尝试加载该类，如果成功则返回类加载结果，否则抛出类未找到异常（ClassNotFoundException）。

### 工作流程示意图

```plaintext
类加载请求
    |
    v
自定义类加载器
    |
    v
扩展类加载器
    |
    v
应用程序类加载器
    |
    v
启动类加载器（Bootstrap ClassLoader）
    |
    v
类加载
```

# 代码示例

以下是一个简单的 Java 类加载器实现双亲委派模型的示例：

```java
public class CustomClassLoader extends ClassLoader {
    public CustomClassLoader(ClassLoader parent) {
        super(parent);
    }

    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
        // 委派给父类加载器
        try {
            return super.loadClass(name);
        } catch (ClassNotFoundException e) {
            // 父类加载器无法加载时，尝试自己加载
            System.out.println("CustomClassLoader loading class: " + name);
            String fileName = name.replace('.', '/') + ".class";
            InputStream is = getClass().getClassLoader().getResourceAsStream(fileName);
            if (is == null) {
                throw new ClassNotFoundException(name);
            }
            try {
                byte[] bytes = new byte[is.available()];
                is.read(bytes);
                return defineClass(name, bytes, 0, bytes.length);
            } catch (IOException e1) {
                throw new ClassNotFoundException(name, e1);
            }
        }
    }
}
```

在这个示例中，`CustomClassLoader` 类实现了一个自定义类加载器，并通过双亲委派模型加载类。首先尝试委派给父类加载器加载类，如果父类加载器无法加载，则由自定义类加载器自己加载。

## 使用场景

双亲委派模型在 Java 的类加载机制中有广泛的应用，主要包括但不限于：

1. **核心类库加载**：如 `java.lang.*` 和 `java.util.*` 等核心类库由启动类加载器加载。
2. **扩展类库加载**：如 `javax.*` 等扩展类库由扩展类加载器加载。
3. **应用程序类库加载**：应用程序的类由应用程序类加载器加载。
4. **自定义类加载**：实现自定义类加载器，加载特定的类或资源，如插件加载、动态代理等。

# Q & A

**Q1: 为什么需要双亲委派模型？**

A1: 双亲委派模型通过层次结构确保核心类库的优先加载，避免类的重复加载和类加载器之间的冲突。它提高了系统的安全性，防止核心类库被篡改和替换。

**Q2: 双亲委派模型如何避免类的重复加载？**

A2: 双亲委派模型通过逐级委托类加载请求，确保每个类只被加载一次。如果一个类已经被父类加载器加载，子类加载器将不再加载该类，从而避免了类的重复加载。

**Q3: 可以破坏双亲委派模型吗？**

A3: 可以通过自定义类加载器破坏双亲委派模型，直接加载特定的类而不委派给父类加载器。尽管这种做法可能满足特定需求，但破坏了类加载机制的安全性和稳定性，需谨慎使用。

**Q4: 双亲委派模型是否适用于所有编程语言？**

A4: 双亲委派模型是 Java 的类加载机制之一，其他编程语言可能有不同的类加载机制。虽然一些语言可能借鉴了双亲委派模型的思想，但实现方式和应用场景可能有所不同。

**Q5: 什么是类加载器的闭包特性？**

A5: 类加载器的闭包特性指的是类加载器加载一个类时，该类所依赖的所有类也由同一个类加载器加载，确保类的依赖关系一致。这一特性确保了类加载器加载的类具有一致的上下文环境。


