---
title: PicoClaw
---

# PicoClaw

- [sipeed/picoclaw](https://github.com/sipeed/picoclaw)
  - MIT, Go
  - by sipeed, 深圳矽速科技有限公司
  - 基于 Go 语言的超高效 AI 助手。
  - 受 NanoBot 启发重构，资源要求极低（<10MB内存，10美元硬件）
  - 支持跨平台运行
- 参考
  - https://picoclaw.io
  - https://docs.picoclaw.io/

```bash
# 从源码构建并安装
git clone https://github.com/sipeed/picoclaw.git
cd picoclaw
make deps
make build
make build-launcher
make install

# 启动 WebUI Launcher
# 在浏览器中打开 http://localhost:18800
picoclaw-launcher

# 命令行交互
picoclaw onboard # 生成 workspace

# 没有 slash 命令，有 exit, quit
picoclaw agent -m "Hello"

# 用于常驻服务，处理 auth callback
# http://127.0.0.1:18790/health
# /ready
# /reload
picoclaw gateway
```

## Configuration

- **主配置文件**: 默认位于 `~/.picoclaw/config.json`
  - 可通过环境变量 `PICOCLAW_CONFIG` 覆盖指定其他路径。
- **数据与工作区根目录**: 默认位于 `~/.picoclaw/workspace/`
  - 可通过环境变量 `PICOCLAW_HOME` 更改数据存放位。

### Workspace

```
~/.picoclaw/workspace/
├── AGENT.md          # Agent 行为准则指南
├── IDENTITY.md       # Agent 身份设定
├── SOUL.md           # Agent 的性格与灵魂
├── USER.md           # 用户的个人偏好信息
├── HEARTBEAT.md      # 定期主动执行的任务（配合 Cron 与 Heartbeat 机制）
├── skills/           # 安装的自定义 .md 技能脚本
├── sessions/         # 会话履历（按渠道/用户隔离）
│   │   # 命名格式: agent_main_{channel}_{scope}_{peer_id}.jsonl
│   ├── agent_main_main.jsonl                              # CLI 默认会话 (cli:default)
│   ├── agent_main_main.meta.json                          # 会话元信息
│   ├── agent_main_weixin_direct_{wechat_id}.jsonl         # 微信用户会话
│   ├── agent_main_telegram_direct_{user_id}.jsonl         # Telegram 用户会话
│   └── agent_main_pico_direct_{connection_id}.jsonl       # Pico/Launcher Web 会话
├── memory/           # 长期记忆存储（跨会话持久）
│   └── MEMORY.md     # 核心记忆文件，Agent 自动读写，记录用户偏好/重要事项
├── state/            # 运行时持久状态
│   └── state.json    # 记录最后活跃的 channel（用于心跳等场景的默认推送目标）
└── cron/             # 定时任务持久化
    └── jobs.json     # Cron 任务数据库（重启后自动恢复）
```

- AGENT.md
  - 角色、职责、能力边界和行为规则
  - 能定义 tools, model, skills, mcpServers
- SOUL.md
  - 沟通语气、价值观、讲话风格
- ~~IDENTITY.md~~ - 现在没有了
- USER.md
  - 用户说明书

```
## Preferences
- Language: 中文
- Timezone: Asia/Shanghai

## Personal Information
- Occupation: 后端工程师
```

**消息结构** - `pkg/agent/context.go` · `BuildMessages()`

```
role=system   ← 一整块系统消息（所有块用 --- 分隔）
role=user     ← 历史对话
role=assistant
role=tool
...
role=user     ← 当前用户输入
```

**System Message 模板**（静态缓存 + 动态追加）

```
[静态，有缓存，mtime 变化自动失效]
① Identity       硬编码：版本、workspace 路径、工具使用规则
② Bootstrap      AGENT.md / SOUL.md / USER.md
③ Skills Summary 技能目录（名称+描述，全文按需 read_file 加载）
④ Memory         MEMORY.md 内容

[动态，每次请求重新生成]
⑤ Dynamic Context  当前时间 / OS / Channel / 发送者 ID
⑥ Active Skills    /use <skill> 激活的技能完整内容
⑦ Context Summary  长对话超过阈值时的压缩摘要
```

> Anthropic 对静态块打 `cache_control: ephemeral`，支持服务端 KV Cache 复用。

## UI & Platforms

- **WebUI Launcher**
  - 提供可视化的网页配置与聊天界面 (`http://localhost:18800`)。
  - **Windows 支持**: 完全原生支持。提供 `picoclaw-launcher.exe`，支持双击直接运行并在系统托盘显示。
  - **macOS / Linux**: 提供原生可执行单文件，同样支持图形环境托盘驻留。
- **Terminal UI (TUI) Launcher**
  - 为无图形界面的服务器（如 SSH 登录）或极简开发板提供的全功能终端控制台界面 (`picoclaw-launcher-tui`)。
- **Android 支持**
  - 目前可以通过 Termux 环境在旧安卓手机作为 Linux 服务器运行，后续规划出原生 APK。

## Notes

- gateway
  - http/websocket
- channel
  - 微信、企业微信、TG、飞书、Discord、Dingtalk
  - 处理为统一的。Message结构
- AgentLoop
  - Turn
  - SubTurn
- Context & Memory
  - JSONL
  - Token budget
- Hooks & EventBus
- Steering - 意图干预
  - 允许在 Agent 进行多步工具调用（Tool calls）推理的间隙，从外部向这个思考 Loop 中注入系统提示词予以纠偏。
- Tools
  - Web (网页抓取与网络搜索，内置百度、Tavily、GLM、DuckDuckGo、SearXNG 等搜索引擎接入)
  - Exec / Shell (命令行执行工具，默认带有一套危险命令拦截正则守卫)
  - Cron (定时任务调度)
  - FileSystem / Edit (本地文件读写与修改操作)
  - Hardware (针对嵌入式板卡硬件的 I2C/SPI 物理总线控制工具)
- Skills
  - 接入 ClawHub 等注册表，进行技能查询与下载
- MCP
  - 动态集成 Model Context Protocol 服务器
  - 支持 Discovery 特性（延迟/按需加载工具，避免撑爆 Token）
- 会话隔离范围
  - 用户
  - 渠道
  - 用户 + 渠道

## API

picoclaw-launcher

- web/frontend
- /api/config
- /api/logs
- /api/skills

# FAQ
