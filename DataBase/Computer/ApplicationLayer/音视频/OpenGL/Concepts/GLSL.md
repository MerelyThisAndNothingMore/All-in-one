---
tags: 
alias:
- OpenGL着色器语言
- OpenGL Shading Language
---
GLSL是为图形计算量身定制的，它包含一些针对向量和矩阵操作的有用特性。

# 基本属性
## 示例
一个典型的[[着色器]]有下面的结构：
```c++
#version version_number
in type in_variable_name;
in type in_variable_name;

out type out_variable_name;

uniform type uniform_name;

int main()
{
  // 处理输入并进行一些图形操作
  ...
  // 输出处理过的结果到输出变量
  out_variable_name = weird_stuff_we_processed;
}
```
每个输入变量也叫顶点属性(Vertex Attribute)。我们能声明的顶点属性是有上限的，它一般由硬件来决定。[[OpenGL]]确保至少有16个包含4分量的顶点属性可用
## 数据类型

GLSL中包含C等其它语言大部分的默认基础数据类型：`int`、`float`、`double`、`uint`和`bool`。GLSL也有两种容器类型，它们会在这个教程中使用很多，分别是向量(Vector)和矩阵(Matrix)

|类型|含义|
|---|---|
|`vecn`|包含`n`个float分量的默认向量|
|`bvecn`|包含`n`个bool分量的向量|
|`ivecn`|包含`n`个int分量的向量|
|`uvecn`|包含`n`个unsigned int分量的向量|
|`dvecn`|包含`n`个double分量的向量|
