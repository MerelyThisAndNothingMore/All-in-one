---
tags: 
alias:
---
# apk组成和Android的打包流程?
## 文件目录
resources.arsc 编译后的二进制资源文件。

classes.dex 是.dex文件。最终生成的Dalvik字节码

AndroidManifest.xml 程序的全局清单配置文件

res是uncompiled resources。存放资源文件的目录。

META-INF是签名文件夹。 存放签名信息

MANIFEST.MF(清单文件):其中每一个资源文件都有一个SHA-256-Digest签名，MANIFEST.MF文件的 SHA256(SHA1)并base64编码的结果即为CERT.SF中的SHA256-Digest-Manifest值。

CERT.SF(待签名文件):除了开头处定义的SHA256(SHA1)-Digest-Manifest值，后面几项的值是对 MANIFEST.MF文件中的每项再次SHA256并base64编码后的值。

CERT.RSA(签名结果文件):其中包含了公钥、加密算法等信息。首先对前一步生成的MANIFEST.MF使用了 SHA256(SHA1)-RSA算法，用开发者私钥签名，然后在安装时使用公钥解密。最后，将其与未加密的摘要信息 (MANIFEST.MF文件)进行对比，如果相符，则表明内容没有被修改。

## 具体打包过程

1.aapt 打包资源文件生成 R.java 文件;
aidl 生成 java 文件 
编译AndroidManifest.xml，
编译生成resources.arsc
2.将 java 文件编译为 class 文件 
3.将工程及第三方的 class 文件转换成 dex 文件 
4.将 dex 文件、so、编译过的资源、原始资源等打包成 apk 文件 
5.签名 
6.资源文件对齐，减少运行时内存

通过AAPT工具进行资源文件 打包，生成R.java、resources.arsc和res文件

通过AIDL工具处理AIDL文件，生成对应的Java接口文件。

通过Java Compiler编译R.java、Java接口文件、Java源文件，生成.class文件。

通过dex命令，将.class文件和第三方库中的.class文件处理生成classes.dex，该过程主要完成Java字节码转换成 Dalvik字节码，压缩常量池以及清除冗余信息等工作。
通过ApkBuilder工具将资源文件、DEX文件打包生成APK文件。 通过Jarsigner工具，利用KeyStore对生成的APK文件进行签名。 
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

