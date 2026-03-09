---
title: Peekaboo
---

# Peekaboo

- [steipete/Peekaboo](https://github.com/steipete/Peekaboo)
  - macOS CLI, MCP
  - author of openclaw
  - 核心功能：
    - 截屏
    - GUI 自动化
    - 结构化菜单发现
    - 多模型供应商集成
    - MCP 服务 (Tools 与 CLI 保持一致)：
      - **视觉感知**: `see`, `image`, `list`
      - **键鼠交互**: `click`, `press`, `type`, `hotkey`, `scroll`, `swipe`, `drag`, `move`
      - **系统管控**: `app`, `window`, `menu`, `menubar`, `dock`, `space`, `dialog`
  - 要求：macOS 15+
- 参考
  - https://docs.openclaw.ai/platforms/mac/peekaboo
  - https://github.com/openclaw/openclaw/blob/main/skills/peekaboo/SKILL.md

```bash
brew install steipete/tap/peekaboo

# MCP
npx -y @steipete/peekaboo

# 截取全屏并在 Retina 缩放率下保存至桌面
peekaboo image --mode screen --retina --path ~/Desktop/screen.png

# 根据 Label 标签查找按钮并直接点击 (采集、识别、点击一步到位)
peekaboo see --app Safari --json-output | jq -r '.data.snapshot_id' | read SNAPSHOT
peekaboo click --on "Reload this page" --snapshot "$SNAPSHOT"

peekaboo agent "看看我飞书现在的 chat"

# ~/.peekaboo/config.json
peekaboo config init

# openai|anthropic|grok|xai|gemini
peekaboo config add openai API_KEY

# 从 env 读取 {env:MY_API_KEY}
# 可以在 ~/.peekaboo/credentials 设置 env
peekaboo config add-provider local \
  --type openai \
  --name "Local Provider" \
  --base-url "http://127.0.0.1:31235/v1" \
  --api-key "x"

peekaboo config test-provider local

# 检查权限
# https://github.com/steipete/Peekaboo/blob/main/docs/permissions.md
peekaboo permissions
peekaboo permissions grant

open "x-apple.systempreferences:com.apple.preference.security?Privacy_ScreenCapture"
```

- PEEKABOO_AI_PROVIDERS
- OPENAI_API_KEY

```json
{
  "mcpServers": {
    "peekaboo": {
      "command": "npx",
      "args": ["-y", "@steipete/peekaboo"],
      "env": {
        "PEEKABOO_AI_PROVIDERS": "openai/gpt-5.1,anthropic/claude-opus-4"
      }
    }
  }
}
```

# FAQ

## 权限

- cli 需要 terminal 有权限

```bash
# cli 触发截屏权限, 可以给 terminal 权限, 例如 iTerm2, Ghostty
/usr/sbin/screencapture
```

- 特殊情况使用 bridge
- 下载 app https://github.com/steipete/Peekaboo/releases 才能赋予权限
