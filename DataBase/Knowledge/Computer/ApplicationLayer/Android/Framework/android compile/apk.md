---
tags: 
alias:
---
# apk组成
**APK 其实是一个 zip 类型的压缩包**
## 文件目录
### AndroidManifest.xml
程序的全局清单配置文件

### classes.dex 
**是由项目源码生成的 .class 文件经过进一步地转换而生成的 Android 系统可识别的 Dalvik Byte Code。并且，由于 Android 系统中的字节码和标准 JVM 中的字节码是有区别的，所以如果 App 中引用了第三方 jar 包的话，那么通常情况下它也会被包含在 classes.dex 中**。

### resources.arsc 
**资源索引表，包含编译后的二进制资源文件**。每当在 **res** 文件夹下放一个文件时，**aapt** 就会自动生成对应的 **id** 并保存在 **.R** 文件中，**但 .R 文件仅仅只是保证编译程序不会报错，实际上在应用运行时，系统会根据 ID 寻找对应的资源路径，而 resources.arsc 文件就是用来记录这些 ID 和 资源文件位置对应关系 的文件**。

### res目录
**未编译的资源文件**。

### asserts
**额外建立的资源文件夹**。**res** 和 **assets** 的不同在于 **res** 目录下的文件会在 **.R** 文件中生成对应的资源 **ID**，而 **assets** 不会自动生成对应的 **ID**，而是通过 **AssetManager** 类的接口来获取。

### libs目录
**如果存在的话，存放的是 ndk 编出来的 so 库** 。

### META-INF目录
**用于保存 App 的签名和校验信息，以保证程序的完整性。当生成 APK 包时，系统会对包中的所有内容做一次校验，然后将结果保存在这里。而手机在安装这一 App 时还会对内容再做一次校验，并和 META-INF 中的值进行比较，以避免 APK 被恶意篡改**。

MANIFEST.MF(清单文件):其中每一个资源文件都有一个SHA-256-Digest签名，MANIFEST.MF文件的 SHA256(SHA1)并base64编码的结果即为CERT.SF中的SHA256-Digest-Manifest值。

CERT.SF(待签名文件):除了开头处定义的SHA256(SHA1)-Digest-Manifest值，后面几项的值是对 MANIFEST.MF文件中的每项再次SHA256并base64编码后的值。

CERT.RSA(签名结果文件):其中包含了公钥、加密算法等信息。首先对前一步生成的MANIFEST.MF使用了 SHA256(SHA1)-RSA算法，用开发者私钥签名，然后在安装时使用公钥解密。最后，将其与未加密的摘要信息 (MANIFEST.MF文件)进行对比，如果相符，则表明内容没有被修改。

# 编译打包流程
- 1、**首先，.aidl（Android Interface Description Language）文件需要通过 aidl 工具转换成编译器能够处理的 Java 接口文件**。

- 2、**同时，资源文件（包括 AndroidManifest.xml、布局文件、各种 xml 资源等等）将被 AAPT（Asset Packaging Tool）（Android Gradle Plugin 3.0.0 及之后使用 AAPT2 替代了 AAPT）处理为最终的 resources.arsc，并生成 R.java 文件以保证源码编写时可以方便地访问到这些资源**。

- 3、**然后，通过 Java Compiler 编译 R.java、Java 接口文件、Java 源文件，最终它们会统一被编译成 .class 文件**。

- 4、**因为 .class 并不是 Android 系统所能识别的格式，所以还需要通过 dex 工具将它们转化为相应的 Dalvik 字节码（包含压缩常量池以及清除冗余信息等工作）。这个过程中还会加入应用所依赖的所有 “第三方库”**。

- 5、**下一步，通过 ApkBuilder 工具将资源文件、DEX 文件打包生成 APK 文件**。
- 6、**接着，系统将上面生成的 DEX、资源包以及其它资源通过 apkbuilder 生成初始的 APK 文件包**。

- 7、**然后，通过签名工具 Jarsigner 或者其它签名工具对 APK 进行签名得到签名后的 APK。如果是在 Debug 模式下，签名所用的 keystore 是系统自带的默认值，否则我们需要提供自己的私钥以完成签名过程**。

- 8、**最后，如果是正式版的 APK，还会利用 ZipAlign 工具进行对齐处理，以提高程序的加载和运行速度。而对齐的过程就是将 APK 文件中所有的资源文件距离文件的起始位置都偏移4字节的整数倍，这样通过 mmap 访问 APK 文件的速度会更快，并且会减少其在设备上运行时的内存占用**。

### assets和res/raw

这两个文件目录里的文件都会直接在打包apk的时候直接打包到apk中，携带在应用里面供应用访问，而且不会 被编译成二进制;
他们的不同点在于: 1、assets中的文件资源不会映射到R中，而res中的文件都会映射到R中，所以raw文件夹下的资源都有对应的ID; 2、assets可以能有更深的目录结构，而res/raw里面只能有一层目录;

3、资源存取方式不同，assets中利用AssetsManager，而res/raw直接利用getResource()， openRawResource(R.raw.fileName),很多人认为是R.id.filename,其实正确的是R.raw.filename,就像 R.drawable.filename一样，整体表示一个ID值，并非是R.id.filename;
# Android的签名机制，签名如何实现的,v2相比于v1签名机制的改变
签名工具

Android 应用的签名工具有两种:jarsigner 和 signAPK。 它们的签名算法没什么区别，主要是签名使用的文件 不同

jarsigner:jdk 自带的签名工具，可以对 jar 进行签名。使用 keystore 文件进行签名。生成的签名文件默认使用 keystore 的别名命名。

signAPK:Android sdk 提供的专门用于 Android 应用的签名工具。 signapk.jar是Android源码包中的一个签名 工具。代码位于Android源码目录下，signapk.jar 可以编译build/tools/signapk/ 得到。 使用 pk8、x509.pem 文件 进行签名。其中 pk8 是私钥文件，x509.pem 是含有公钥的文件。生成的签名文件统一使用“CERT”命名。

jarsigner和apksigner的区别

Android提供了两种对Apk的签名方式，一种是基于JAR的签名方式，另一种是基于Apk的签名方式，它们的主要 区别在于使用的签名文件不一样: jarsigner使用keystore文件进行签名; apksigner除了支持使用keystore文件进行 签名外，还支持直接指定pem证书文件和私钥进行签名。

签名过程

APK是先摘要，再签名

  要了解如何实现签名，需要了解两个基本概念:数字摘要和数字证书。

数字摘要

就是对消息数据，通过一个Hash算法计算后，都可以得到一个固定长度的Hash值，这个值就是消息摘要。

特征: 唯一性 固定长度:比较常用的Hash算法有MD5和SHA1，MD5的长度是128拉，SHA1的长度是160位。 不可逆性

  消息摘要只能保证消息的完整性，并不能保证消息的不可篡改性
数字签名

在摘要的基础上再进行一次加密，对摘要加密后的数据就可以当作数字签名。利用非对称加密技术，通过私钥对 摘要进行加密，产生一个字符串，如RSA就是常用的非对称加密算法。在没有私钥的前提下，非对称加密算法能确保 别人无法伪造签名，因此数字签名也是对发送者信息真实性的一个有效证明。不过由于Android的keystore证书是自 签名的，没有第三方权威机构认证，用户可以自行生成keystore，Android签名方案无法保证APK不被二次签名。

签名和校验的主要过程


选取一个签名后的 APK(Sample-release.APK)解压,在 META-INF 文件夹下有三个文件:MANIFEST.MF、 CERT.SF、CERT.RSA。它们就是签名过程中生成的文件

MANIFEST.MF  
逐一遍历 APK 中的所有条目，如果是目录就跳过，如果是一个文件，就用 SHA1(或者 SHA256)消息摘要算法

提取出该文件的摘要然后进行 BASE64 编码。 分别用Name和SHA1-Digest记录

签名过程:

1、计算摘要:  
通过Hash算法提取出原始数据的摘要。  
2、计算签名: 再通过基于密钥(私钥)的非对称加密算法对提取出的摘要进行加密，加密后的数据就是签名信息。

3、写入签名:

  将签名信息写入原始数据的签名区块内。

校验过程:

签名验证是发生在APK的安装过程中

1、计算摘要

首先用同样的Hash算法从接收到的数据中提取出摘要。

2、解密签名:

  使用发送方的公钥对数字签名进行解密，解密出原始摘要。

3、比较摘要:

  如果解密后的数据和提取的摘要一致，则校验通过;如果数据被第三方篡改过，解密后的数据和摘要将会不一
致，则校验不通过。


# APK的安装流程
复制APK到/data/app目录下，解压并扫描安装包。 
资源管理器解析APK里的资源文件。 解析AndroidManifest文件，并在/data/data/目录下创建对应的应用数据目录。 
然后针对[[Dalvik]]/[[ART]]环境优化dex文件，保存到dalvik-cache目录
将AndroidManifest文件解析出的四大组件和权限信息注册到PackageManagerService中。
安装完成后，发送广播。

