---
title: sub2api
---

# sub2api

- [Wei-Shaw/sub2api](https://github.com/Wei-Shaw/sub2api)
  - LGPL-3.0, Go, Vue 3, Vite, Ent, PostgreSQL, Redis
  - Sub2API-CRS2 一站式开源 AI API 中转服务，让 Claude、Openai 、Gemini、Antigravity 订阅统一接入，支持拼车共享和成本分摊。
- 参考
  - [官网](https://sub2api.org)
  - [Demo](https://demo.sub2api.org/)

```bash
docker run -d --name sub2api \
  -p 8080:8080 \
  -v $(pwd)/data:/app/data \
  -e DATABASE_URL=postgres://sub2api:password@localhost:5432/sub2api \
  -e REDIS_URL=redis://localhost:6379 \
  -e JWT_SECRET=$(openssl rand -hex 32) \
  -e TOTP_ENCRYPTION_KEY=$(openssl rand -hex 32) \
  weishaw/sub2api:latest
```

# FAQ

### 配置

- 环境变量 `RUN_MODE=simple` 可进入简易模式。
- `JWT_SECRET` 和 `TOTP_ENCRYPTION_KEY` 推荐手动生成以保持状态。
- 通过 Nginx 反向代理时，需开启 `underscores_in_headers on;` 以支持 `session_id`。

### Antigravity 集成

支持专用端点：

- `/antigravity/v1/messages` -> Claude
- `/antigravity/v1beta/` -> Gemini
