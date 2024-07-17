---
tags: 
alias:
- Uniform Resource Identifier
- 统一资源标识符
---

# 定义

**URI（Uniform Resource Identifier，统一资源标识符）** 是一种用于标识资源的字符串。URI 提供了一个统一的方式来标识和访问资源，这些资源可以是文档、图像、服务、电子邮件地址等。URI 的定义和使用在网络和互联网应用中非常普遍。

## 特点

1. **唯一性**：
   - URI 提供了一种统一且唯一的方式来标识资源，每个 URI 都唯一地标识一个特定的资源。
   
2. **结构化**：
   - URI 具有特定的结构，通常包含方案（scheme）、主机（host）、路径（path）、查询（query）和片段（fragment）等部分。

3. **可扩展性**：
   - URI 的结构设计使其具有很强的可扩展性，能够适应各种类型的资源和协议。

4. **通用性**：
   - URI 可以用来标识网络上的各种资源，不仅限于 web 页面，还包括文件、服务、应用程序、数据库等。

## 相似概念辨析

1. **URI vs URL**：
   - URL（Uniform Resource Locator，统一资源定位符）是 URI 的一个子集，专门用于标识资源的位置。URL 通常包含访问资源所需的具体地址和协议。
   
2. **URI vs URN**：
   - URN（Uniform Resource Name，统一资源名称）也是 URI 的一个子集，用于标识资源的名称，不必指示资源的位置或如何访问。URN 更侧重于资源的持久性和唯一性。

3. **URI vs IRI**：
   - IRI（Internationalized Resource Identifier，国际化资源标识符）是 URI 的扩展，允许使用非 ASCII 字符，以支持国际化。

# 原理

URI 通过特定的语法规则来标识资源，其基本结构如下：

```
scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]
```

- **scheme**：指定访问资源所使用的协议或方法，例如 `http`、`https`、`ftp`、`mailto` 等。
- **host**：指定资源所在的主机名或 IP 地址。
- **port**：指定访问资源所使用的端口号（可选）。
- **path**：指定资源在主机上的具体路径。
- **query**：指定资源的查询参数，通常用于动态请求（可选）。
- **fragment**：指定资源的片段或子资源（可选）。

# 使用

URI 在网络和互联网应用中广泛使用，包括但不限于以下场景：

1. **Web 页面**：
   - 通过 URL 访问网页，例如 `https://www.example.com/index.html`。
   
2. **API 请求**：
   - 使用 URI 访问和操作 Web 服务或 API，例如 `https://api.example.com/v1/users?id=123`。
   
3. **资源链接**：
   - 在文档或应用程序中嵌入资源链接，例如图片、视频、文档等。
   
4. **电子邮件地址**：
   - 使用 `mailto:` 方案标识电子邮件地址，例如 `mailto:example@example.com`。

# 示例

以下是几个常见的 URI 示例，展示了不同类型的资源标识：

1. **网页 URL**：
   - `https://www.example.com/path/to/resource?query=example#fragment`
     - `scheme`: https
     - `host`: www.example.com
     - `path`: /path/to/resource
     - `query`: query=example
     - `fragment`: fragment

2. **FTP URL**：
   - `ftp://user:password@ftp.example.com/path/to/file`
     - `scheme`: ftp
     - `user:password`: user:password
     - `host`: ftp.example.com
     - `path`: /path/to/file

3. **电子邮件地址**：
   - `mailto:example@example.com`
     - `scheme`: mailto
     - `address`: example@example.com

4. **数据库连接 URI**：
   - `mongodb://dbuser:dbpassword@host:port/dbname`
     - `scheme`: mongodb
     - `user:password`: dbuser:dbpassword
     - `host:port`: host:port
     - `path`: /dbname

# Q & A

1. **什么是 URI？**
   - URI 是一种用于标识资源的字符串，通过统一的方式来标识和访问资源。

2. **URI 和 URL 有什么区别？**
   - URL 是 URI 的一个子集，专门用于标识资源的位置和访问方法。所有的 URL 都是 URI，但并不是所有的 URI 都是 URL。

3. **URI 的基本结构是什么？**
   - URI 的基本结构包括 scheme、host、path、query 和 fragment 等部分，具体格式取决于资源类型和协议。

4. **URI 在哪些场景中使用？**
   - URI 在网络和互联网应用中广泛使用，包括访问网页、API 请求、资源链接和电子邮件地址等。

通过理解 URI 的定义、特点、原理和实际应用，可以更好地设计和使用 URI 来标识和访问各种网络资源，提高系统的互操作性和灵活性。


