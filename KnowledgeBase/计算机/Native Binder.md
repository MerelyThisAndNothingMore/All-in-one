---
tags: 
alias:
---

除了常规C/S架构的Client端和Server端，还包括了ServiceManager，它用于管理系统中的服务。  
首先Server进程会注册一些Service到ServiceManager中，Client要使用某个Service，则需要先到ServiceManager查询Service的相关信息，然后根据Service的相关信息与Service所在的Server进程建立通信通路，这样Client就可以使用Service了。
