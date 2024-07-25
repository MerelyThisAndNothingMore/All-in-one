---
tags: 
alias:
---

# 定义

OKHttp 是一个用于 [[Android]] 和 [[Java]] 应用程序的高效 [[HTTP]] & HTTP/2 客户端。它由 Square 开发和维护，旨在提供简单、可靠且高效的 HTTP 请求和响应处理功能。

## 特点

1. **异步和同步请求**：支持同步和异步的 HTTP 请求方式，满足不同场景的需求。
2. **连接池**：通过连接池复用 HTTP 连接，提高请求效率和性能。
3. **支持 HTTP/2**：提供对 HTTP/2 的支持，允许多路复用和减少延迟。
4. **缓存**：内置缓存机制，提高响应速度并减少网络请求。
5. **拦截器**：支持请求和响应的拦截器，便于对请求和响应进行统一处理和修改。
6. **自动重试**：在网络请求失败时，支持自动重试机制，提高请求的可靠性。

## 工作原理

OKHttp 的工作原理主要涉及以下几个组件：

1. **OkHttpClient**：HTTP 客户端，负责配置和管理 HTTP 请求的具体实现。
2. **Request**：HTTP 请求，包含请求的 URL、头信息和请求体等。
3. **Response**：HTTP 响应，包含响应的状态码、头信息和响应体等。
4. **Interceptor**：拦截器，用于在请求和响应的各个阶段进行拦截和处理。

### 工作流程

1. 创建 OkHttpClient 对象。
2. 构建 Request 对象，指定请求的 URL 和参数。
3. 通过 OkHttpClient 发起请求，获取 Response 对象。
4. 处理响应，获取响应的状态码、头信息和响应体。

## 示例代码

以下是一个使用 OKHttp 发送 GET 和 POST 请求的示例代码：

### 发送 GET 请求

```java
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class OkHttpExample {
    public static void main(String[] args) {
        OkHttpClient client = new OkHttpClient();

        Request request = new Request.Builder()
            .url("https://api.github.com/")
            .build();

        try (Response response = client.newCall(request).execute()) {
            if (response.isSuccessful()) {
                System.out.println(response.body().string());
            } else {
                System.out.println("Request failed: " + response.message());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 发送 POST 请求

```java
import okhttp3.MediaType;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;
import okhttp3.Response;

public class OkHttpExample {
    public static final MediaType JSON = MediaType.get("application/json; charset=utf-8");

    public static void main(String[] args) {
        OkHttpClient client = new OkHttpClient();

        String json = "{\"name\":\"John\", \"age\":30}";
        RequestBody body = RequestBody.create(json, JSON);
        Request request = new Request.Builder()
            .url("https://jsonplaceholder.typicode.com/posts")
            .post(body)
            .build();

        try (Response response = client.newCall(request).execute()) {
            if (response.isSuccessful()) {
                System.out.println(response.body().string());
            } else {
                System.out.println("Request failed: " + response.message());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 拦截器

OKHttp 支持自定义拦截器，可以在请求和响应的不同阶段进行拦截和处理。例如，可以在拦截器中添加公共的头信息，记录日志，或者实现重试机制。

### 自定义拦截器示例

```java
import java.io.IOException;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class OkHttpExample {
    public static void main(String[] args) {
        OkHttpClient client = new OkHttpClient.Builder()
            .addInterceptor(new Interceptor() {
                @Override
                public Response intercept(Chain chain) throws IOException {
                    Request request = chain.request();
                    Request newRequest = request.newBuilder()
                        .header("User-Agent", "OkHttp Example")
                        .build();
                    return chain.proceed(newRequest);
                }
            })
            .build();

        Request request = new Request.Builder()
            .url("https://api.github.com/")
            .build();

        try (Response response = client.newCall(request).execute()) {
            if (response.isSuccessful()) {
                System.out.println(response.body().string());
            } else {
                System.out.println("Request failed: " + response.message());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 使用场景

OKHttp 广泛应用于需要进行 HTTP 请求的各种场景，主要包括但不限于：

1. **网络请求**：发送 GET、POST 等 HTTP 请求，获取服务器数据。
2. **文件上传和下载**：支持大文件的上传和下载，提供断点续传和进度监控功能。
3. **数据同步**：与服务器进行数据同步，保持数据一致性。
4. **API 调用**：调用各种 RESTful API，获取和提交数据。
5. **WebSocket**：支持 WebSocket 协议，实现实时通信。

## Q & A

**Q1: OKHttp 与 HttpURLConnection 有什么区别？**

A1: OKHttp 是一个第三方库，提供了更高效和功能丰富的 HTTP 客户端，相比 HttpURLConnection 提供了更简单易用的 API 和更多的功能，如连接池、HTTP/2 支持、拦截器、缓存等。HttpURLConnection 是 Java 标准库的一部分，功能较为基础，使用相对复杂。

**Q2: OKHttp 如何处理重试机制？**

A2: OKHttp 内置了自动重试机制，对于某些常见的网络错误（如连接超时、连接重置等），会自动进行重试。可以通过自定义拦截器实现更复杂的重试逻辑，如设置最大重试次数、根据错误类型进行不同的重试策略等。

**Q3: OKHttp 如何处理 HTTPS 请求？**

A3: OKHttp 默认支持 HTTPS 请求，并会自动处理 SSL/TLS 握手。如果需要自定义 SSL 配置，可以通过 OkHttpClient.Builder 的 `sslSocketFactory` 方法设置自定义的 SSL 套接字工厂和信任管理器。

**Q4: OKHttp 是否支持 HTTP/2？**

A4: 是的，OKHttp 默认支持 HTTP/2 协议，允许多路复用和减少延迟，从而提高请求效率和性能。

**Q5: OKHttp 如何进行文件上传和下载？**

A5: OKHttp 通过 RequestBody 和 ResponseBody 实现文件上传和下载。可以使用 MultipartBody 进行多部分上传，实现文件和其他数据的同时上传。下载文件时，可以通过读取 ResponseBody 的输入流，将数据写入本地文件。





