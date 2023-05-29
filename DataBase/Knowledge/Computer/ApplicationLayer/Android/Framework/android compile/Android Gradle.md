---
tags: 
alias:
---
# 介绍
标准的 As 项目中, 包含三大部分:

- Top-level Gradle:用于配置所有的 Module 属性
- Moudle-level Gradle: 配置独立 Moudle 的属性
- Gradle Wrapper: 用于统一编译环境, 一般供 CI 使用

## 关键概念

### script

gradle script 都是 configuration scripts. 这话的意思是说, 运行起来后, 每个脚本文件最终都会对应到一个程序中的对象, 这个对象叫做 delegate object. 比如, build.gradle 对应为程序中的 `Project`.

