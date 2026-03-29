---
title: PicoClaw
---

# PicoClaw

- [sipeed/picoclaw](https://github.com/sipeed/picoclaw)
  - MIT, Go
  - by sipeed
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
picoclaw-launcher
# 在浏览器中打开 http://localhost:18800

# 命令行交互
picoclaw onboard
picoclaw agent -m "Hello"
```

## Configuration

- **主配置文件**: 默认位于 `~/.picoclaw/config.json`
  - 可通过环境变量 `PICOCLAW_CONFIG` 覆盖指定其他路径。
- **数据与工作区根目录**: 默认位于 `~/.picoclaw/workspace/`
  - 可通过环境变量 `PICOCLAW_HOME` 更改数据存放位。

### Workspace

- `AGENT.md`: Agent 行为准则指南
- `IDENTITY.md`: Agent 身份设定
- `SOUL.md`: Agent 的性格与灵魂
- `USER.md`: 用户的个人偏好信息
- `HEARTBEAT.md`: 存放让 Agent 定期主动执行的任务列表（配合 Cron 与 Heartbeat 机制）
- `skills/`: 存放安装的自定义 `.md` 技能脚本
- `sessions/`: 会话履历
- `memory/`: 长期记忆存储（如 `MEMORY.md`）

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

# FAQ
