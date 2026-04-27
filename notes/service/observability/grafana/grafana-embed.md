---
title: Grafana Embedding
---

# Grafana Embedding

- [grafana/scenes](https://github.com/grafana/scenes)
  - Apache-2.0, TypeScript
  - A frontend framework for building dashboard-like experiences (the modern way to embed Grafana components).
- 参考
  - [Share a dashboard or panel](https://grafana.com/docs/grafana/latest/visualizations/dashboards/share-dashboards-panels/)
  - [Embed Grafana panels using iframes](https://grafana.com/docs/grafana/latest/panels-visualizations/visualizations/text/#embed-grafana-panels-using-iframes)
  - [Public dashboards](https://grafana.com/docs/grafana/latest/dashboards/snapshots-public-dashboards/public-dashboards/)

# Embedding Methods

## 1. Iframe (Standard)

最常用的嵌入方式，通过 `<iframe>` 嵌入 `dashboard-solo` 路径。

```html
<iframe
  src="https://grafana.example.com/d-solo/uuid/name?orgId=1&panelId=2"
  width="100%"
  height="300"
  frameborder="0"
></iframe>
```

**URL 参数:**

- `panelId`: 指定嵌入的面板 ID。
- `from`, `to`: 时间范围。
- `theme`: `light` 或 `dark`。
- `kiosk`: 隐藏侧边栏和顶栏（通常 `d-solo` 已经只包含面板内容）。

## 2. Public Dashboards

可以直接公开访问，不需要 Grafana 账号，安全性较低，主要用于非敏感公开数据。

## 3. Grafana Scenes (React App)

如果是开发复杂的 React 应用，推荐使用 Grafana Scenes 库，它允许你像组合 React 组件一样使用 Grafana 的面板、数据源和时间轴。

# Configuration

嵌入外部网站通常需要修改 `grafana.ini`:

```ini
[security]
# 允许嵌入 Iframe (移除 X-Frame-Options Header)
allow_embedding = true

# 如果应用和 Grafana 不在同一个主域名下，需要设置 None
# 注意: 设置为 None 必须同时设置 cookie_secure = true (HTTPS)
cookie_samesite = none
cookie_secure = true

[auth.anonymous]
# 如果需要免登录访问，开启匿名访问
enabled = true
org_role = Viewer
```

# Authentication

1. **Anonymous Access**: 全局开启，最简单，但无法区分用户。
2. **Auth Proxy**: 通过 Nginx/Reverse Proxy 注入 `X-WEBAUTH-USER` Header，适合内部系统集成。
3. **JWT**: 通过 Header 传递 JWT Token，Grafana 验证 Token 的合法性。
4. **JWT URL Parameter**: v9.1+ 支持 `?aut_token=<jwt>` 参数。

# FAQ

### Refused to display in a frame because X-Frame-Options is set to 'deny'

设置 `allow_embedding = true`。

### 嵌入后提示登录，即使设置了匿名访问

检查浏览器 Cookie 设置。如果是跨域嵌入且没有 HTTPS，浏览器会阻止 `SameSite=None` 的 Cookie。

- 确保 Grafana 使用 HTTPS。
- 设置 `cookie_samesite = none` 和 `cookie_secure = true`。

### CSP 问题

如果外部网站有 CSP 限制，需要添加 `frame-src` 允许 Grafana 域名。
如果反代（Nginx）设置了 CSP，确保 `frame-ancestors` 允许你的目标网站域名。
