---
title: Vanguard
---

# Vanguard

- [connectrpc/vanguard-go](https://github.com/connectrpc/vanguard-go)
  - Apache-2.0, Go, Connect, gRPC, REST
  - Vanguard 是一个用于 Go `net/http` 服务器的库，支持在 REST、gRPC、gRPC-Web 和 Connect 协议之间进行无缝转码。

# 支持的转换场景

Vanguard 的 `Transcoder` 充当了“全能翻译官”，支持在以下四种协议之间进行**任意两两转换**：

| 协议         | 说明                                                  |
| :----------- | :---------------------------------------------------- |
| **Connect**  | 由 Buf 提出的现代 RPC 协议，支持 HTTP/1.1 和 HTTP/2。 |
| **gRPC**     | 标准 gRPC 协议，通常需要 HTTP/2。                     |
| **gRPC-Web** | 为浏览器设计的 gRPC 变体，支持 HTTP/1.1。             |
| **REST**     | 基于 `google.api.http` 注解的 JSON/HTTP 接口。        |

# 使用示例

```go
import (
    "connectrpc.com/vanguard"
    "net/http"
)

func main() {
    // 假设 proxy 是一个反向代理到后台 gRPC 服务的 handler
    handler, _ := vanguard.NewTranscoder([]*vanguard.Service{
        vanguard.NewService("my.Service", proxy),
    })
    http.ListenAndServe(":8080", handler)
}
```

# 参考

- [Connect RPC 官网](https://connectrpc.com/)
- [Vanguard-Go GitHub](https://github.com/connectrpc/vanguard-go)
