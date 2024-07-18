---
tags: 
aliases:
  - Shared Object File
---

## 定义

.so 文件（共享对象文件，Shared Object File）是Unix和类Unix[[操作系统]]（如Linux）中使用的一种动态库文件格式。它们通常包含可以在运行时被多个程序共享的代码和数据。类似于Windows中的DLL（动态链接库，Dynamic Link Library）文件，.so 文件允许程序在运行时动态加载和链接库函数。

## 特点

1. **共享性**：多个程序可以同时使用同一个 .so 文件，节省内存空间。
2. **动态链接**：程序在运行时加载 .so 文件，可以减少程序的启动时间和内存使用。
3. **版本控制**：可以通过不同版本的 .so 文件来管理库的升级和兼容性问题。
4. **模块化**：通过 .so 文件，可以将程序的不同功能模块化，便于开发和维护。
5. **跨语言调用**：支持使用多种编程语言编写的代码，通过 .so 文件进行交互。

## 相似概念辨析

### .so 文件 vs 静态库

1. **链接方式**：
   - **.so 文件**：动态链接，程序在运行时加载。
   - **静态库**：静态链接，程序在编译时将库代码直接包含在可执行文件中。

2. **内存使用**：
   - **.so 文件**：多个程序共享同一个 .so 文件，节省内存。
   - **静态库**：每个程序都包含一份库代码，占用更多内存。

3. **更新维护**：
   - **.so 文件**：库更新时，只需替换 .so 文件，无需重新编译依赖程序。
   - **静态库**：库更新时，必须重新编译依赖程序。

### .so 文件 vs DLL 文件

1. **操作系统**：
   - **.so 文件**：主要用于Unix和类Unix操作系统（如Linux）。
   - **DLL 文件**：主要用于Windows操作系统。

2. **文件格式**：
   - **.so 文件**：遵循ELF（Executable and Linkable Format）格式。
   - **DLL 文件**：遵循PE（Portable Executable）格式。

3. **命名约定**：
   - **.so 文件**：通常以 `.so` 作为文件扩展名，可能包含版本号，例如 `libexample.so.1.0`。
   - **DLL 文件**：以 `.dll` 作为文件扩展名，例如 `example.dll`。

## 原理

共享对象文件的原理是通过操作系统的动态链接器（如 `ld-linux.so`）在程序运行时加载和链接所需的共享库。具体步骤如下：

1. **编译阶段**：将源代码编译为目标文件（.o 文件），然后使用 `ld` 工具将多个目标文件链接成一个 .so 文件。
2. **链接阶段**：在程序编译时指定需要链接的 .so 文件，编译器会记录这些依赖关系，但不会将库代码包含在可执行文件中。
3. **运行时加载**：程序启动时，动态链接器根据程序中的依赖信息加载所需的 .so 文件，并将库函数的地址绑定到程序中。

### 示例：创建和使用 .so 文件

#### 创建 .so 文件

1. 编写源代码：

```c
// example.c
#include <stdio.h>

void hello() {
    printf("Hello, Shared Object!\n");
}
```

2. 编译生成 .so 文件：

```sh
gcc -fPIC -c example.c
gcc -shared -o libexample.so example.o
```

#### 使用 .so 文件

1. 编写使用 .so 文件的程序：

```c
// main.c
#include <stdio.h>

void hello(); // 声明外部函数

int main() {
    hello();
    return 0;
}
```

2. 编译并链接 .so 文件：

```sh
gcc -o main main.c -L. -lexample
```

3. 运行程序：

```sh
export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH
./main
```

## 使用

1. **模块化开发**：将程序的功能模块分割为多个 .so 文件，便于开发和维护。
2. **插件系统**：通过 .so 文件实现插件系统，动态加载和卸载功能模块。
3. **跨语言调用**：使用 .so 文件实现不同编程语言之间的代码共享和调用。
4. **性能优化**：通过动态链接库优化程序的启动时间和内存使用。

## Q & A

**Q1: .so 文件如何解决版本兼容性问题？**

A1: .so 文件通常通过文件名中的版本号来管理不同版本。例如，`libexample.so.1.0` 表示第一个大版本的第一个小版本。程序可以通过符号链接（symbolic link）引用特定版本的 .so 文件，如 `libexample.so -> libexample.so.1.0`。

**Q2: 如何在编译时指定 .so 文件的路径？**

A2: 可以在编译时使用 `-L` 选项指定 .so 文件的路径，例如 `-L/path/to/lib`，并使用 `-l` 选项指定库名，例如 `-lexample`。还可以使用 `LD_LIBRARY_PATH` 环境变量在运行时指定 .so 文件的搜索路径。

**Q3: .so 文件的主要优势是什么？**

A3: .so 文件的主要优势包括节省内存、减少程序的启动时间、简化库的更新和维护、支持模块化开发和跨语言调用等。

**Q4: 如何查看 .so 文件包含的符号信息？**

A4: 可以使用 `nm` 命令查看 .so 文件包含的符号信息，例如 `nm -D libexample.so`。还可以使用 `readelf` 或 `objdump` 命令获取更多详细信息。

**Q5: .so 文件如何在程序运行时动态加载？**

A5: 可以使用 `dlopen`、`dlsym` 和 `dlclose` 函数在程序运行时动态加载 .so 文件。例如：

```c
#include <dlfcn.h>

void* handle = dlopen("libexample.so", RTLD_LAZY);
void (*hello)() = dlsym(handle, "hello");
hello();
dlclose(handle);
```

