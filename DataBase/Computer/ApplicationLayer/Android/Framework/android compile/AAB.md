---
tags: 
aliases:
  - Android App Bundle
---

# 概述
**定义**：AAB是Google推出的新型应用发布格式，它更加智能和高效。

**打包方法**：
- 在Android Studio中，通过点击菜单栏的`Build > Build Bundle(s) / APK(s) > Build Bundle(s)`来生成AAB。
- 使用Gradle命令行工具，运行`./gradlew bundleRelease`（或`gradlew.bat bundleRelease`在Windows上）。

**特点**：
- AAB包含了应用的所有必要资源和代码，但不是最终用户安装的格式。
- Google Play使用AAB来生成并提供针对用户设备优化的[[APK]]，称为动态交付。
- 减少了用户下载应用的大小，提高了安装率和下载速度。
- 支持模块化和按需交付，允许应用在需要时下载特定功能或资源。
