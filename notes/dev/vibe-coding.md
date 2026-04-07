---
title: Vibe Coding
tags:
  - Thoughs
---

# Vibe Coding

:::tip

- 如果你 review 了所有的代码，那就不算是 vibe coding。
- AI 无法完全完成 当事人 也无法理解的任务。
- 让 Agent 能 Loop 起来是关键，因此人类的主要工作是
  - 准备工具 - 例如 访问浏览器、访问日志、访问数据
    - 对应 MCP, tools
  - 准备环境 - 例如 测试环境、.env 配置、Terminal 环境
    - 对应 项目配置
  - 准备上下文 - 例如 描述现象，描述触发逻辑，描述需求，描述验收
    - 对应 skills, AGENTS.md, rules
  - 准备工作流 - 例如 自动知乎回答问题流程
    - 对应 skills, subagent
    - 对于 Coding 类任务 AI 已经非常熟悉 工作流程
- 工程中最难的不是“造出一个能用的东西”，而是以正确的方式构建它，让它易于理解，也易于扩展。
  - 但 过早的优化是万恶之源
  - 过于 "right" 的架构设计得到可能是不需要的产出
  - Make it DONE, Make it Right, Make it Fast

:::

- 明确项目需求和范围
- 建立全面的设计指南和编码标准
- 记录所有约束和限制
- 创建并维护包含信息的 markdown 文件，便于客户端访问
- 只有在完成上述步骤后再启动编码过程
- OODA - Observe Orient Decide Act - 观察 认知 决策 执行

## 如何增加并行 {#how-to-increase-parallel}

1. use worktree
2. use tmux
3. use session name

- the /rename command

**worktree**

- ~/gits/foo
  - main repo
- ~/gits/foo-1
  - workrtree
  - symlink CLAUDE.md, .mcp.json, .claude, .agents, local
- 可快速增加并行程度，不丢失上下文
- 通过组织目录可 share 上下文

**tmux**

```
prefer tmux to run dev server, use title to find the target pane like `xyz-dev`
```

with [tmux-session-manager](https://github.com/wenerme/ai/tree/main/skills/tmux-session-manager) skill

## Share Context

- 让模型知道你知道的
- 让你知道模型知道的

我的习惯性结构

```
.agents/
  docs/       # 让模型持续更新跟踪相关知识上下文
  skills/     # 基础技能
local/
  docs/       # 我应该需要知道的
  YYYY-MM-DD/ # 模型应该知道的，每次的上下文
```

- 知识技能梳理 https://github.com/wenerme/ai

## Best Practice

- Simplicity is always the best
- 使用 Worktree 增加并行 - Poor man's Team or Parallel Agent
  - 使用 softlink 共享文件 - .claude, .mcp.json, CLAUDE.md 配置等
  - 使用 local/ 本地共享目录来维护本地共享知识和本地开发用数据和各种 case
  - 使用 `local/docs/*` 来跟踪相关文档
- 规划自己的 prompt 历史记录跟踪书写内容
- 确保能 Handoff
  - 完整的环境、配置、测试条件
- Command line first
  - 很多工作，command line 工具能更好胜任
  - argocd, kubectl, git, glab, gh, agent-browser, mcp-cli

## 提示词

:::tip

- 使用伪代码驱动 AI 进行编码
- 先 Plan 再编码

:::

- 为待完成的工作提供详细的规格说明
- 必要时包含相关上下文和文件
- 针对具体任务有策略性地应用提示，便于审查和测试
- 将大型任务拆解为更小、更聚焦的子任务，以获得更好结果

---

- 对每次更改进行增量测试
- 尽可能实现自动化测试
- 根据原始需求进行验证
- 维护完善的测试套件（CI/CD）
- 定期进行自动化安全和质量扫描

---

- 明确制定代码生成和修改的规则
- 各环境保持一致的配置
- 记录特殊规则和例外情况
- 定期回顾和更新配置设置
- 实施模块化设计原则

## 参考

- 2025-11 Claude Opus 4.5 标志着 AI Codeing Agent 从 Copilot -> Coworker 的转变
- Time to Edit (TTE)
- https://cognition.ai/blog/devin-annual-performance-review-2025
- https://cursor.com/blog/productivity
- https://news.ycombinator.com/item?id=46515696
