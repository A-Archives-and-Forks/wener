---
title: Casdoor
---

# Casdoor

- [casdoor/casdoor](https://github.com/casdoor/casdoor)
  - Apache-2.0, Go, React, Beego, Xorm
  - Casdoor 是一款基于 Casbin 权限引擎、UI 优先的现代联邦身份和身份访问管理（IAM）单点登录系统。
  - casbin authz, xorm
  - DB: PostgreSQL, MySQL, SQL Server, SQLite
  - 前端: React 18, AntD, ECharts
  - Redis 同步、Session、RateLimit
- 默认账号 admin:123

## NOTES

| Category        | 说明               | Type                                                                                                                                                                                                            |
| :-------------- | :----------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OAuth           | 联邦身份与社交登录 | adfs, alipay, azuread_b2c, baidu, bilibili, casdoor, custom, dingtalk, douyin, facebook, gitee, github, gitlab, google, goth, kwai, lark, linkedin, metamask, okta, qq, telegram, twitter, wechat, wecom, weibo |
| Email           | 邮件发送服务       | azure_acs, custom_http, resend, sendgrid, smtp                                                                                                                                                                  |
| SMS             | 短信发送服务       | Aliyun, Tencent Cloud, Huawei Cloud, VolcEngine, Baidu Cloud, Twilio, Azure ACS, PNVS, Custom HTTP                                                                                                              |
| Storage         | 存储管理 (OSS)     | aliyun_oss, aws_s3, azure, casdoor, cucloud_oss, google_cloud, local_file_system, minio_s3, synology_nas, tencent_cloud_cos, qiniu_cloud                                                                        |
| Payment         | 支付网关           | adyen, airwallex, alipay, balance, dummy, fastspring, gc, lemonsqueezy, paddle, paypal, polar, stripe, wechatpay                                                                                                |
| Captcha         | 验证码服务         | aliyun, default, geetest, hcaptcha, recaptcha, turnstile                                                                                                                                                        |
| SAML / OIDC     | 标准联邦协议       | Keycloak, Okta, OneLogin (via Custom SAML/OIDC)                                                                                                                                                                 |
| LDAP            | 身份目录集成       | Active Directory, OpenLDAP, OpenDJ                                                                                                                                                                              |
| WebAuthn        | 无密码/生物认证    | Passkeys, FIDO2                                                                                                                                                                                                 |
| FaceId          | 活体人脸识别       | aliyun, tencent_cloud                                                                                                                                                                                           |
| ID Verification | 实名认证 (IDV)     | aliyun, jumio                                                                                                                                                                                                   |
| Log             | 日志与 Agent 审计  | bark, cucloud, custom_http, dingtalk, discord, google_chat, lark, line, matrix, microsoft_teams, pushbullet, pushover, reddit, rocket_chat, slack, telegram, twitter, viber, webpush, wecom, OpenClaw           |
| Radius          | 网络接入认证       | Radius Client / Server                                                                                                                                                                                          |

- `conf/app.conf` 作为主配置文件，控制端口、DB、语言、域名等所有环境变量。
- `init_data.json` 提供种子数据（如默认 Admin 账户、built-in 组织）。运行二进制文件（`/server --createDatabase=true`）时会自动完成建库加载。
- /swagger/
- /.well-known/jwks
- /api/login/oauth/access_token
- /api/enforce
  - AuthZ
- Service Account = Client Credentials
- https://door.casdoor.com/swagger

## config

- conf/app.conf

```ini
# --- 基础 Web 配置 ---
appname = casdoor
# 服务监听端口
httpport = 8000
# 运行模式：dev (开发模式) 或 prod (生产模式)
runmode = dev
# 是否允许获取请求体（Beego 内部使用）
copyrequestbody = true
# 静态资源基础 URL (用于 CDN 映射)
staticBaseUrl = "https://cdn.casbin.org"

# --- 数据库配置 ---
# 支持: mysql, postgres, mssql, sqlite
driverName = postgres
# 连接字符串 (注意不同驱动格式不同)
dataSourceName = "user=casdoor password=xxx host=localhost port=5432 sslmode=disable dbname=casdoor"
# 数据库名
dbName = casdoor
# 表前缀 (可选)
tableNamePrefix =
# 是否在控制台显示 SQL 执行语句
showSql = false

# --- 中间件配置 ---
# Redis 连接地址，留空则使用文件系统/内存存储
redisEndpoint = "127.0.0.1:6379"
# SOCKS5 代理地址 (用于访问某些社交登录服务)
socks5Proxy = "127.0.0.1:10808"

# --- 业务逻辑配置 ---
# Casdoor 实例对外的访问路径 (影响 Token ISS 指标和回调)
origin = "http://localhost:8000"
# 前端文件存放路径
frontendBaseDir = "web/build"
# 验证码过期时间 (分钟)
verificationCodeTimeout = 10
# 默认语言
defaultLanguage = "en"
# 默认应用 (内置 Admin 使用)
defaultApplication = "app-built-in"

# --- 日志配置 (JSON 格式) ---
# adapter 可选: console, file
logConfig = {"adapter":"file", "filename": "logs/casdoor.log", "maxdays":99999, "perm":"0770"}

# --- 其他 (LDAP / Radius) ---
ldapServerPort = 389
radiusServerPort = 1812
radiusSecret = "secret"

# --- 扩展/配额配置 ---
quota = {"organization": -1, "user": -1, "application": -1, "provider": -1}
```
