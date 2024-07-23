---
tags: 
alias:
---


在 [[Android]] 开发中，使用 [[Gradle]] 进行构建和依赖管理时，可以通过不同的依赖模式来管理模块之间的依赖关系。常用的依赖模式包括 `api`、`implementation`、`compileOnly` 和 `runtimeOnly`。了解这些模式的作用和区别，有助于优化项目的依赖管理，提高构建效率。

## 依赖模式

### `api`

- **作用**：将依赖添加到项目中，并将这些依赖传递给其他依赖该模块的模块。
- **应用场景**：当一个模块中的[[类]]需要公开给其他模块使用时，使用 `api` 依赖模式。
- **特点**：依赖项不仅对本模块可见，对依赖本模块的其他模块也可见。

**示例**：

```groovy
dependencies {
    api 'com.squareup.retrofit2:retrofit:2.9.0'
}
```

在这个示例中，`retrofit` 库将对当前模块和所有依赖该模块的模块可见。

### `implementation`

- **作用**：将依赖添加到项目中，但不将这些依赖传递给其他依赖该模块的模块。
- **应用场景**：当一个模块中的依赖只在本模块内部使用，不需要公开给其他模块时，使用 `implementation` 依赖模式。
- **特点**：依赖项仅对本模块可见，对依赖本模块的其他模块不可见。

**示例**：

```groovy
dependencies {
    implementation 'com.google.code.gson:gson:2.8.6'
}
```

在这个示例中，`gson` 库将仅对当前模块可见，而对其他依赖该模块的模块不可见。

### `compileOnly`

- **作用**：将依赖添加到编译时，但不在运行时包含这些依赖。
- **应用场景**：当一个模块需要在编译时引用某些类，但在运行时不需要这些类时，使用 `compileOnly` 依赖模式。例如，接口定义或[[注解]]处理器。
- **特点**：依赖项仅在编译时可见，不会包含在最终的 APK 中。

**示例**：

```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.20'
}
```

在这个示例中，`lombok` 库仅在编译时可见，而不会包含在最终的 APK 中。

### `runtimeOnly`

- **作用**：将依赖添加到运行时，但不在编译时包含这些依赖。
- **应用场景**：当一个模块需要在运行时加载某些类，但在编译时不需要这些类时，使用 `runtimeOnly` 依赖模式。例如，数据库驱动或插件。
- **特点**：依赖项仅在运行时可见，不会在编译时引用。

**示例**：

```groovy
dependencies {
    runtimeOnly 'org.postgresql:postgresql:42.2.18'
}
```

在这个示例中，`postgresql` 库仅在运行时可见，而不会在编译时引用。

### `annotationProcessor`

- **作用**：用于添加注解处理器的依赖，这些依赖在编译时处理注解。
- **应用场景**：当一个模块使用注解处理器生成代码时，使用 `annotationProcessor` 依赖模式。
- **特点**：依赖项仅在编译时用于处理注解，不会包含在最终的 APK 中。

**示例**：

```groovy
dependencies {
    annotationProcessor 'com.google.dagger:dagger-compiler:2.35.1'
}
```

在这个示例中，`dagger-compiler` 库仅在编译时用于处理注解，不会包含在最终的 APK 中。

## 使用示例

以下是一个综合使用各种依赖模式的 `build.gradle` 示例：

```groovy
dependencies {
    // 公开依赖，对所有模块可见
    api 'com.squareup.retrofit2:retrofit:2.9.0'
    
    // 内部依赖，仅对当前模块可见
    implementation 'com.google.code.gson:gson:2.8.6'
    
    // 编译时依赖，不会包含在 APK 中
    compileOnly 'org.projectlombok:lombok:1.18.20'
    
    // 运行时依赖，不会在编译时引用
    runtimeOnly 'org.postgresql:postgresql:42.2.18'
    
    // 注解处理器依赖，仅在编译时处理注解
    annotationProcessor 'com.google.dagger:dagger-compiler:2.35.1'
}
```

在这个示例中，不同的依赖模式用于不同的场景，确保依赖管理的高效性和模块化。

## 总结

- **`api`**：公开依赖，对所有依赖本模块的模块可见。
- **`implementation`**：内部依赖，仅对当前模块可见。
- **`compileOnly`**：编译时依赖，不包含在运行时。
- **`runtimeOnly`**：运行时依赖，不包含在编译时。
- **`annotationProcessor`**：注解处理器依赖，仅在编译时处理注解。

通过合理使用这些依赖模式，可以优化项目的依赖管理，减少编译时间和 APK 体积，提高开发效率。
