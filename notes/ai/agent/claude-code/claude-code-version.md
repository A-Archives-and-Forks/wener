---
tags:
  - Version
---

# Version

| version | date       | notes |
| ------- | ---------- | ----- |
| 2.1     | 2026-01-07 |
| 2.0     | 2025-09-29 |

- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://claudelog.com/claude-code-changelog/

| env                                     | since  | demo                     |
| --------------------------------------- | ------ | ------------------------ |
| CLAUDE_CODE_TMPDIR                      | 2.1.5  |
| CLAUDE_CODE_DISABLE_BACKGROUND_TASKS    | 2.1.4  |
| CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS | 2.1.0  |
| CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS    | 2.1.32 |
| ENABLE_TOOL_SEARCH                      | 2.1.8  | auto,`auto:N`,true,false |
| CLAUDE_CODE_PLUGIN_SEED_DIR             | 2.1.63 |
| ENABLE_CLAUDEAI_MCP_SERVERS             | 2.1.63 |

- ENABLE_TOOL_SEARCH
  - mcp tool 占用超过 10% 上下文就会自动开启
  - 200k -> 20k
- ENABLE_EXPERIMENTAL_MCP_CLI
  - 动态发现 tool
  - 使用 mcp-cli call server/tool 的方式来调用 tool

## Claude Code 2.1.32

- CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS 启用实验性 Team
- 会加载 --add-dir 的 .claude/skills/
- skill 最多占用 2% 上下文，会随着上下文增加加载内容

## Claude Code 2.1.20

- CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD
  - 支持读取 --add-dir 目录下的 CLAUDE.md

## Claude Code 2.1.17

- TOOD -> Task
  - 支持依赖
  - 支持多个共同使用
  - CLAUDE_CODE_TASK_LIST_ID
  - ~/.claude/tasks
  - ~/.claude/tasks/ID/N.json
  - 以前的 ~/.claude/todos/ID.json

```json title="~/.claude/tasks/ID/N.json"
{
  "id": "3",
  "subject": "",
  "description": "",
  "activeForm": "",
  "status": "completed",
  "blocks": [],
  "blockedBy": []
}
```

```json title="~/.claude/todos/ID.json"
[
  {
    "content": "",
    "status": "completed",
    "activeForm": ""
  }
]
```

## Claude Code 2.1

- 技能热重载
  - ~/.claude/skills
  - .claude/skills
- skill `context: fork` to run skill in forked sub-agent context
- `list_changed` notification to reload mpc tools
- 支持 `language` 设置
- `Shift+Enter` 能正常使用
- permission Bash 支持 `*` 通配符在任意位置
  - 例如 `Bash(git * main)`
  - 之前必须 `:*` 例如 `Bash(git checkout:*)`
- `Ctrl+B` 能同时控制 Bash 和 Agent 在后台运行
- `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` 能控制文件读取的输出 token 数
