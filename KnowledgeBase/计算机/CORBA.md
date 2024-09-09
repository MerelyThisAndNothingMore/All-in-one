---
tags: 
aliases:
  - Common Object Request Broker Architecture
---
## 定义

**CORBA**（Common [[对象|Object]] Request Broker Architecture，公共对象请求代理架构）是一种由**OMG（Object Management Group）**定义的标准，用于使应用程序可以在分布式网络环境中进行交互。CORBA使不同平台、[[编程语言]]的应用程序能够互相通信，简化了分布式系统的开发。

通过CORBA，客户端可以通过标准化接口调用位于其他服务器或设备上的对象，而无需关心对象所在的设备、编程语言或操作系统。

## 特点

1. **跨平台性**：CORBA支持不同操作系统（如Windows、Linux、Unix）之间的通信。
2. **跨语言性**：CORBA允许使用不同编程语言的应用程序（如C++、Java、Python）进行交互。
3. **透明的对象调用**：通过ORB（Object Request Broker，对象请求代理），客户端可以像调用本地对象一样调用远程对象。
4. **分布式计算支持**：CORBA用于构建分布式应用，使得对象可以在不同的机器上进行交互。
5. **接口定义语言（IDL）**：CORBA使用IDL来定义对象的接口，确保跨语言的互操作性。

## 相似概念辨析

### CORBA vs RMI（Remote Method Invocation）
- **CORBA**：支持跨语言和跨平台的远程调用。
- **RMI**：Java特有的远程调用机制，支持Java对象之间的远程通信，但不能跨语言。

### CORBA vs SOAP
- **CORBA**：基于二进制协议，性能较高，适合复杂的分布式系统。
- **SOAP**：基于XML的消息传输协议，通常用于基于Web服务的分布式系统，具有更高的通用性和开放性，但效率较低。

### CORBA vs gRPC
- **CORBA**：较老的分布式系统标准，支持跨语言通信。
- **gRPC**：基于HTTP/2和Protocol Buffers的新兴RPC框架，广泛用于现代微服务架构，性能优异，支持多语言。

## 原理

CORBA的核心是**ORB（Object Request Broker，对象请求代理）**，它负责在客户端和服务器端之间传递对象调用。客户端通过ORB发起对远程对象的调用，ORB负责找到远程对象、执行调用并将结果返回给客户端。

### CORBA架构的关键组件：

1. **ORB**：负责管理客户端和服务器之间的通信，隐藏分布式系统的复杂性。
2. **IDL（Interface Definition Language）**：用于定义分布式对象的接口，使得不同编程语言的对象能够互操作。
3. **Object Adapter**：将远程对象注册到ORB，并管理对象的生命周期。
4. **Stub和Skeleton**：
   - **Stub**：客户端调用时，通过Stub将调用信息传递给ORB。
   - **Skeleton**：在服务器端，Skeleton接收ORB传来的请求，并调用实际的对象方法。

## 使用

### 1. 创建IDL接口

首先，使用CORBA的IDL定义对象的接口：

```idl
interface Hello {
    string sayHello();
}
```

IDL定义的接口可以通过编译器生成不同语言的客户端和服务器端代码。

### 2. 编写服务器端实现

服务器端实现IDL接口，提供远程对象的具体功能：

```java
public class HelloImpl extends HelloPOA {
    public String sayHello() {
        return "Hello, CORBA!";
    }
}
```

### 3. 启动ORB和注册对象

服务器启动时，创建ORB实例并注册远程对象：

```java
ORB orb = ORB.init(args, null);
HelloImpl helloRef = new HelloImpl();
orb.connect(helloRef);
```

### 4. 客户端调用远程对象

客户端通过ORB找到远程对象并调用方法：

```java
ORB orb = ORB.init(args, null);
Hello hello = HelloHelper.narrow(orb.resolve_initial_references("HelloService"));
System.out.println(hello.sayHello());
```

## Q & A

### 1. **CORBA的主要优势是什么？**
   CORBA的主要优势在于其跨平台和跨语言的能力，特别适用于复杂的分布式系统。

### 2. **为什么CORBA在现代应用中不常用了？**
   尽管CORBA在早期广泛使用，但随着Web服务和轻量级RPC框架（如gRPC、REST）的兴起，CORBA因其复杂性和配置困难而逐渐被替代。

### 3. **CORBA如何处理对象的生命周期？**
   通过Object Adapter，CORBA可以管理对象的生命周期，包括创建、销毁和持久化等。

## 总结

CORBA是一种强大的分布式系统架构标准，允许不同语言和平台之间的对象互操作。它使用IDL定义接口，通过ORB管理远程调用的透明性。尽管其复杂性和维护成本较高，但在某些高性能、跨平台的分布式系统中，CORBA仍然有一定的应用场景。然而，随着更简便的分布式技术（如gRPC、SOAP等）的兴起，CORBA的使用逐渐减少。