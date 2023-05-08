---
tags: 
alias:
---
1.ARouter 路由表生成原理

@Route注解，会在编译时期通过apt生成一些存储path和activityClass映射关系的类文件

APT是Annotation Processing Tool的简称,即注解处理工具。它是在编译期对代码中指定的注解进行解析，然后 做一些其他处理(如通过javapoet生成新的Java文件)

第一步:定义注解处理器，用来在编译期扫描加入@Route注解的类，然后做处理。 这也是apt最核心的一步， 新建RouterProcessor 继承自 AbstractProcessor,然后实现process方法。在项目编译期会执行RouterProcessor的 process()方法，我们便可以在这个方法里处理Route注解了


第二步，在process()方法里开始生成EaseRouter_Route_moduleName类文件和EaseRouter_Group_moduleName文 件。这里在process()里生成文件用javapoet生成java文件，就会用 JavaPoet 生成 Group、Provider 和 Root 路 由文件，路由表就是由这些文件组成的，内容loadInto方法通过传入一个特定类型的map就能把分组信息放入map里为，一个 map(其实是两个map，一个保存group列表，一个保存group下的路由地址和activityClass关系)保存了路由地址和 ActivityClass的映射关系，然后通过map.get("router address") 拿到AncivityClass，通过startActivity() 调用就好了


2.ARouter 路由表加载原理 https://github.com/Xiasm/EasyRouter/wiki/%E6%A1%86%E6%9E%B6%E7%9A%84%E5%88%9D%E5%A7%

8B%E5%8C%96

app进程启动的时候会拿到这些类文件，把保存这些映射关系的数据读到内存里(保存在map里)到这些类文件便 可以得到所有的routerAddress---activityClass映射关系 去扫描apk中所有的dex，遍历找到所有包名为 packageName的类名，然后将类名再保存到classNames集合里

1. 读取apk中所有的dex文件 2.然后判断类的包名是否为 “com.alibaba.android.arouter.routes”，获取到注 解处理器生成的类名时，就会把这些类名保存 SharedPreferences 中，下次就根据 App 版本判断，如果不 是新版本，就从本地中加载类名，否则就用 ClassUtils 读取类名。 3.就会根据类名的后缀判断类是 IRouteRoot 、IInterceptorGroup 还是 IProviderGroup ，然后根据不同的类把类文件的内容加载到索引 中。获取到映射关系

3.ARouter 跳转原理 路由跳转的时候，通过build()方法传入要到达页面的路由地址，ARouter会通过它自己存储的路由表找到路由地

址对应的Activity.class(activity.class = map.get(path))，然后new Intent() 1.在build的时候，传入要跳转的路由地址，build()方法会返回一个Postcard对象，我们称之为跳卡。然后调用

Postcard的navigation()方法完成跳转 2.ARouter会通过它自己存储的路由表找到路由地址对应的Activity.class(activity.class = map.get(path))，然后

new Intent()