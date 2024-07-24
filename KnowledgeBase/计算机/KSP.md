---
tags: 
aliases:
  - Kotlin Symbol Processing
---

# 定义

KSP（Kotlin Symbol Processing）是 JetBrains 提供的一种新的工具，用于在 [[Kotlin]] 中进行编译时代码生成。KSP 类似于 [[Java]] 的[[注解处理器]]（Annotation Processing），但它是为 Kotlin 量身定制的，旨在为 Kotlin 提供更高效和更灵活的编译时处理支持。

## 特点

1. **高效性**：KSP 直接处理 Kotlin 源代码符号，不需要将 Kotlin 代码转换为 Java 抽象语法树（AST），提高了处理效率。
2. **灵活性**：KSP 提供了一个简单且功能强大的 API，使得编写编译时代码生成器更加灵活和直观。
3. **无缝集成**：KSP 与 Kotlin [[编译器]]无缝集成，支持多模块项目和增量编译。
4. **广泛支持**：KSP 支持所有 Kotlin 语言特性，包括扩展函数、委托属性等。

## 结构

KSP 的工作流程主要包括以下几个部分：

1. **符号处理器（Symbol Processor）**：实现 `SymbolProcessor` 接口，定义代码生成逻辑。
2. **符号处理器提供器（SymbolProcessorProvider）**：实现 `SymbolProcessorProvider` 接口，提供符号处理器的实例。
3. **符号（Symbol）**：KSP 提供一套 API 用于访问和操作 Kotlin 代码中的符号，如类、函数、属性等。

### 类图示例

```plaintext
+---------------------+
| SymbolProcessor     |
| +process()          |
+---------------------+
          ^
          |
+---------------------+
| MySymbolProcessor   |
+---------------------+

+---------------------+
| SymbolProcessorProvider |
| +create()                |
+---------------------+
          ^
          |
+---------------------+
| MySymbolProcessorProvider |
+---------------------+
```

# 示例

### 定义符号处理器

首先，需要定义一个符号处理器，实现 `SymbolProcessor` 接口：

```kotlin
import com.google.devtools.ksp.processing.*
import com.google.devtools.ksp.symbol.*

class MySymbolProcessor(private val env: SymbolProcessorEnvironment) : SymbolProcessor {
    override fun process(resolver: Resolver): List<KSAnnotated> {
        val symbols = resolver.getSymbolsWithAnnotation("com.example.MyAnnotation")
        symbols.forEach { symbol ->
            if (symbol is KSClassDeclaration) {
                // 处理类符号
                env.logger.info("Found class: ${symbol.qualifiedName?.asString()}")
                // 生成代码逻辑
            }
        }
        return emptyList()
    }
}
```

### 定义符号处理器提供器

然后，需要定义一个符号处理器提供器，实现 `SymbolProcessorProvider` 接口：

```kotlin
class MySymbolProcessorProvider : SymbolProcessorProvider {
    override fun create(environment: SymbolProcessorEnvironment): SymbolProcessor {
        return MySymbolProcessor(environment)
    }
}
```

### 配置 KSP

在 `build.gradle.kts` 文件中配置 KSP：

```kotlin
plugins {
    kotlin("jvm") version "1.5.21"
    id("com.google.devtools.ksp") version "1.5.21-1.0.0"
}

dependencies {
    implementation("com.google.devtools.ksp:symbol-processing-api:1.5.21-1.0.0")
}

ksp {
    arg("option1", "value1")
}
```

### 使用注解

最后，使用注解在代码中标记需要处理的类：

```kotlin
package com.example

@Target(AnnotationTarget.CLASS)
annotation class MyAnnotation

@MyAnnotation
class MyClass {
    // 类内容
}
```

## 使用场景

KSP 主要用于以下场景：

1. **编译时代码生成**：如生成类型安全的路由代码、数据绑定代码等。
2. **注解处理**：如生成依赖注入框架的代码。
3. **代码分析和检查**：在编译时对代码进行静态分析和检查，提供编译时错误提示。

# Q & A

**Q1: KSP 与 KAPT 有什么区别？**

A1: KSP 是为 Kotlin 设计的符号处理工具，而 KAPT 是 Kotlin 注解处理工具，主要用于兼容 Java 的注解处理器。KSP 直接处理 Kotlin 源代码符号，更高效、更灵活，不需要将 Kotlin 代码转换为 Java AST。

**Q2: 如何调试 KSP 处理器？**

A2: 可以在符号处理器中使用 `env.logger` 打印日志，或在符号处理器中设置断点，通过调试工具进行调试。

**Q3: KSP 支持哪些 Kotlin 语言特性？**

A3: KSP 支持所有 Kotlin 语言特性，包括扩展函数、委托属性、[[内联函数]]等。

**Q4: 如何在多模块项目中使用 KSP？**

A4: 在多模块项目中，可以在每个模块的 `build.gradle.kts` 文件中配置 KSP，并在模块间设置适当的依赖关系，确保符号处理器能够访问到需要处理的符号。

**Q5: KSP 是否支持增量编译？**

A5: 是的，KSP 支持增量编译，提高了编译效率。KSP 通过追踪符号的变化，仅处理变化的部分符号，从而减少编译时间。