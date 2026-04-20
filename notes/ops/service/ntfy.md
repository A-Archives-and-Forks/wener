---
title: ntfy
---

# ntfy

- [binwiederhier/ntfy](https://github.com/binwiederhier/ntfy)
  - Apache-2.0, Go
  - Send push notifications to your phone or desktop using PUT/POST
  - 支持双向
- 参考
  - https://docs.ntfy.sh/subscribe/cli/
  - iOS https://apps.apple.com/us/app/ntfy/id1625396347
  - Google Play https://play.google.com/store/apps/details?id=io.heckel.ntfy

```yaml
version: '3.8'

services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    command:
      - serve
    restart: unless-stopped
    ports:
      - '80:80'
    environment:
      - TZ=Asia/Shanghai
      # 配置 SQLite 缓存以持久化消息
      - NTFY_CACHE_FILE=/var/cache/ntfy/cache.db
      - NTFY_CACHE_DURATION=12h
      # - NTFY_BASE_URL=http://ntfy.example.com
    volumes:
      - ./var/cache/ntfy:/var/cache/ntfy
      # server.yml
      - ./etc/ntfy:/etc/ntfy
```

- 默认内存缓存 12h
- 使用 SQLite 缓存 NTFY_CACHE_FILE=/var/cache/ntfy/cache.db
- cache-duration: 0 可关闭缓存
- attachment-cache-dir 可指定附件缓存位置

## 配置

- `NTFY_BASE_URL` -> `base-url`
- [官方 Config 文档](https://docs.ntfy.sh/config/)

```yaml
# ====== 1. 基础配置 ======
# 你的自建外网访问地址 (重要, 很多功能依赖于此)
base-url: 'https://ntfy.example.com'
# HTTP 监听地址
listen-http: ':80'
# 可选: HTTPS 监听及证书
# listen-https: ":443"
# key-file: "/etc/ntfy/privkey.pem"
# cert-file: "/etc/ntfy/fullchain.pem"

# ====== 2. 缓存与持久化 ======
# SQLite 缓存数据库文件（用于离线消息找回）
cache-file: '/var/cache/ntfy/cache.db'
# 缓存消息最大留存时间 (默认 12h, 设为 0 完全关闭缓存)
cache-duration: '12h'

# ====== 3. 附件配置 ======
# 附件物理缓存目录（不存放在数据库中）
attachment-cache-dir: '/var/cache/ntfy/attachments'
# 全局附件总容量上限 (默认 5G)
# attachment-total-size-limit: "5G"
# 单个附件大小上限 (默认 15M)
# attachment-file-size-limit: "15M"
# 附件到期删除时间 (默认 3h)
# attachment-expiry-duration: "3h"

# ====== 4. 访问控制与权限 ======
# SQLite 用戶权限数据库文件 (开启后才能管理账号与 ACL)
# auth-file: "/var/cache/ntfy/user.db"
# 默认未验证的访客权限: read-write (默认) / read-only / write-only / deny-all
# auth-default-access: "deny-all"
# 开启通过 Web UI 或 API 开放注册
# enable-signup: false
# 开启登录机制
# enable-login: false
# 要求全部请求强制登录 (私有化必备)
# require-login: false

# ====== 5. 访问频率限制 (Rate Limits) ======
# 单个 IP 每秒请求上限等机制 (默认机制适合公开服务，私有化可适当改大)
# visitor-request-limit-burst: 60
# visitor-request-limit-replenish: "5s"
# 单个访客可以订阅的主题数 (默认 30)
# visitor-subscription-limit: 30
# 单个访客每天最高发送多少条消息 (0 = 不受限)
# visitor-message-daily-limit: 0
# 如果套用在反代后面，需开启通过 X-Forwarded-For 判定真实 IP
# behind-proxy: true

# ====== 6. 通知渠道外发接入 ======
# 配置发送 Email 的 SMTP 服务
# smtp-sender-addr: "smtp.example.com:465"
# smtp-sender-user: "admin@example.com"
# smtp-sender-pass: "password"
# smtp-sender-from: "ntfy@example.com"

# 配置 Web Push (主流浏览器系统级推送机制，需生成 VAPID keys)
# web-push-file: "/var/cache/ntfy/webpush.db"
# web-push-email-address: "admin@example.com"
# web-push-public-key: "..."
# web-push-private-key: "..."
```

# FAQ

## ntfy vs Gotify

两者都是非常优秀的自托管消息推送服务，主要区别在于**核心架构模型**和**平台支持**：

| 特性             | ntfy                                                                                                                   | Gotify                                                                                                                                    |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------- |
| **设计模型**     | **Pub-Sub (发布订阅)** 基于话题 (Topic)。无需提前创建应用，只要知道包含某种规律或密钥的 Topic 名称即可收发消息。       | **App-Client (应用-客户)** 模型。必须先在后台创建一个 "Application" 生成 token 才能发消息，然后必须作为一个 "Client" 登录以接收所有消息。 |
| **iOS 支持**     | **良好**。官方在 iOS 上上架了 App，并通过官方的中继服务器来绕过 Apple 的严格推送限制（也支持完全自建的轮询推送机制）。 | **极差/无官方支持**。由于 Apple 要求推送必须通过 APNs 中继服务，Gotify 目前没有提供官方 iOS 应用，也没有提供官方中心化节点转发机制。      |
| **Android 支持** | 支持 FCM (Firebase) 统一下发，也支持全后台 WebSocket 轮询推送（无需 Google 服务框架/F-Droid 版本）。                   | 同样提供包含 FCM / 无后台轮询等多种 Android 客户端选择。                                                                                  |
| **易用性**       | 极简。`curl -d "hi" ntfy.sh/mytopic` 开放性随意发（除非配置了严格的 ACL）。                                            | 偏向于正规的后台管理，所有推送通道都受到后台强制管理的 Token 控制。                                                                       |
| **UI 与功能**    | 支持更多富文本操作、内置动作按钮 (Action Buttons)、附件图片等高级功能，界面偏现代。                                    | 界面比较传统经典，偏向纯文本与 Markdown，不支持附件图片和复杂 Action Button 交互。                                                        |

**总结建议**：

1. **如果你有 iOS 设备**，想最无脑地收到推送消息并且需要发送带附件、带按钮的消息，请无脑选择 **ntfy**。
2. 如果你的全套环境需要严格的前置权限分配，只想单纯发文本，不用 iOS 端，**Gotify** 更老牌经典，隔离性更强。
