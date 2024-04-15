---
tags: 
aliases:
  - Executable and Linkable Format
---

一种广泛使用的文件格式，用于定义程序、库文件（如.so共享库）、核心转储和其他在类Unix系统中执行的文件。它是由UNIX System V发展而来的，现在已经成为[[Linux]]、[[Android]]等[[操作系统]]的标准可执行文件格式。

ELF文件可以分为两种视图：连接视图（Linking View）和执行视图（Execution View）。这两种视图反映了文件在不同阶段的数据组织方式。

### 连接视图（Linking View）

连接视图是指ELF文件在被加载到内存执行前的组织形式。在这个视图中，文件是按照“section”组织的。Section是ELF文件的构建块，每个section包含了执行程序所需的不同类型的数据或指令集，比如代码（text section）、初始化数据（data section）、未初始化数据（bss section）、符号表（symbol table）、调试信息等。

连接视图主要由链接器（Linker）使用，链接器负责处理这些sections，解决符号引用、重定位等问题，并最终生成可执行文件或另一个对象文件。在这个阶段，sections中的信息被用来组合和链接程序，但这些信息不一定按照最终在[[内存]]中的布局来组织。

### 执行视图（Execution View）

执行视图是指ELF文件被加载到内存中后的组织形式。在这个视图中，文件是按照“segment”组织的。Segment是一组sections的集合，这些sections在运行时需要被当作一个单元处理。每个segment定义了一组在运行时需要加载到内存中的sections，以及如何加载它们（例如，是否可读、可写、可执行等属性）。

执行视图主要由操作系统的加载器（Loader）使用，加载器负责读取ELF文件的程序头表（Program Header Table），这个表描述了各个segment的信息，如起始地址、内存布局、权限等。加载器根据这些信息，将程序加载到内存中，准备执行。

