---
tags: 
alias:
---
SharedPreference是Android系统中一种简单的、轻量级的文件存储，它是一种持久化的存储方式，以名称/值对（NVP）机制存放在xml中map根标签下，正如其名，它比较适合一些简单数据的存储，用于保存Int、long、boolean、String、Float、Set这些数据类型，可以在data/data/应用程序/shared_prefs的目录下可以查找到保存的xml文件。

  
# 优化方向
-   建议在Application中初始化，重写attachBaseContext()，参数context直接传入Application对象即可。
-   最好使用单例，不必每次都从系统获取Sp对象，减少开销。
-   如果项目中使用了MultiDex，存在分包，请在分包前即MultiDex.install()之前或者在multidex执行的这段时间初始化。因为这时cpu是利用不满的，我们没有办法充分利用CPU的原因，是因为如果我们在Multidex之前执行一些操作，我们很有可能因为这样一些操作的类或者是相关的类不在我们的主dex当中，在四点几版本中会直接崩溃，但是由于sharePreference不会产生这种崩溃，它是系统的类。
-   请不要使用SharedPreference存储大文件及存储大量的key和value，这样的话会造成界面卡顿或者ANR比较占内存，记住它是简单存储，如果有类似的需求请考虑数据库、磁盘文件存储等等。
-   推荐使用apply进行存储，这也是官方推荐，当读入内存后，因为它是异步写入磁盘的，所以效率上会比commit好，如果你需要存储状态或者即存即用的话还是尽量使用commit。
-   请不要频繁使用apply与commit，如果存在这样的问题，请合并一次性apply与commit,可以参考封装一个map的结合，一次性提交，因为SharedPreference可能会存在IO瓶颈和锁性能差的问题。
-   尽量不要存放Json及html，数据少可以，无需担心，大量的话请放弃。
-   不要所有的数据都存在一个文件，不同类型的文件或者数据可以分开多个文件存储，避免使用一个大文件，这样可提高读取速度。
-   跨进程操作不要使用MULTI_PROCESS标志，而是使用contentprovide等进程间通信的方式。



