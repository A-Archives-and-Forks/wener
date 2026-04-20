---
tags:
  - FAQ
---

# Agent FAQ

## evo

- https://github.com/NousResearch/hermes-agent-self-evolution
  - DSPy + GEPA (Genetic-Pareto Prompt Evolution)

```
skill/prompt
↓ 读取执行 trace（真实失败案例）
↓ LLM 反思"为什么失败"
↓ 生成多个变体（mutation）
↓ 评估每个变体（多目标）
↓ 帕累托筛选
↓ 下一代
```

用 AI 来自动改写 AI 的操作手册（skills/prompts），让 agent 在实际使用中越来越好用

## 什么是 Agent {#what-is-agent}

> 如何把一个会生成文本的模型，变成一个能稳定完成任务、持续工作、可控可追踪的执行系统

1. 它知道该做什么吗？

- 目标怎么进来
  - System Prompt
  - Session 上下文
  - agent definition
- 任务怎么拆
- 优先级怎么定

2. 它知道怎么做吗？

- prompt / skill / tool / policy 怎么组合
- 能力边界怎么定义

3. 它能安全地做吗？

- 权限
- sandbox
- approval
- 错误恢复
- 防止乱调用、乱写、乱删

4. 它能持续做吗？

- session
- memory
- context 管理
- 中断后恢复
- 子任务 / subagent / 异步回流

5. 它能被理解和改进吗？

- 日志
- trace
- 评估
- 回放
- benchmark
- 版本化和演化

不是“让模型回答问题”，而是同时解决：

- 规划问题
- 执行问题
- 状态问题
- 协调问题
- 约束问题
- 评估问题

## skip permissions

```bash
claude --dangerously-skip-permissions

codex --sandbox danger-full-access --ask-for-approval never
codex --dangerously-bypass-approvals-and-sandbox
```

- claude
  - --allow-dangerously-skip-permissions
    - --permission-mode bypassPermissions
    - Shift+Tab 可以切换
  - --dangerously-skip-permissions
- gemini
  - https://github.com/google-gemini/gemini-cli/issues/4340

## Notification

- 任务完成后通知
- macOS terminal-notifier
- Linux notify-send

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "idle_prompt",
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -title '🔔 Claude Code: Input Needed' -message 'Claude is waiting for your input'"
          }
        ]
      }
    ]
  }
}
```

## IDE vs Agent

- 类似 Cursor, Antigravity 其实更接近 Agent 而非 IDE
  - IDE 功能由 VSC 提供
  - Cursor 和 Antigravity 在这基础上提供了 Agent 功能
- 工具范式转移
- IDE 输出=思考 × 编码速度
- Agent 输出=决策 × AI 执行力

> 代码写的越少，效率越高

| 特性     | 传统 IDE (VS Code / IntelliJ) | AI Agent (Cursor / Windsurf)           |
| -------- | ----------------------------- | -------------------------------------- |
| 交互模式 | 被动 (Passive)                | 主动 (Proactive)                       |
| 核心动作 | 编辑 (Edit)、保存、调试       | 规划 (Plan)、生成、执行                |
| 上下文   | 此时此刻打开的文件            | 整个代码库 (Project Awareness)         |
| 人类角色 | 驾驶员 (Driver)               | 指挥官 (Commander) / 审核员 (Reviewer) |
| 输出物   | 字符 (Characters)             | Diff (变更集)                          |

- I (Integrated) -> Intelligent (智能)
  - 死工具 -> 活 Agent / 有认知的工具
- D (Development) -> Delegation (委派)
  - 操作员 -> 指挥官
- E (Environment) -> Entity (实体)
  - 被动 -> 主动
