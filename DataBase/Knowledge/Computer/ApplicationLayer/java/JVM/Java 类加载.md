---
tags: 
alias:
---

### 类加载的本质
将描述类的数据 从`Class`文件加载到内存 & 对数据进行校验、转换解析 和 初始化，最终形成可被虚拟机直接使用的`Java`使用类型

> `Class`文件是一串二进制字节流

### 类加载过程
![](https://upload-images.jianshu.io/upload_images/944365-31072687a32f8861.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
加载 -> 验证 -> 准备 -> 解析 -> 初始化
![](https://upload-images.jianshu.io/upload_images/944365-6f1d148aa90ea914.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

# 步骤1：加载
![](https://upload-images.jianshu.io/upload_images/944365-248b92b723ae3aa6.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

# 步骤2：验证
![](https://upload-images.jianshu.io/upload_images/944365-09f423a9f9e47af7.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤3：准备
![](https://upload-images.jianshu.io/upload_images/944365-f700de8a0d2b4ef0.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤4：解析
![](https://upload-images.jianshu.io/upload_images/944365-ae7602d88369eae8.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤5：初始化
![](https://upload-images.jianshu.io/upload_images/944365-3064969a380dde0b.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)











