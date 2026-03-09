---
title: Anthropic
---

# Anthropic

| date       | flag                                       | for                                 | provider | fields                                                                                                                                     |
| ---------- | ------------------------------------------ | ----------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 2024-07-31 | [prompt-caching-2024-07-31]                | Prompt caching breakpoints          | A,B,V,F  | `*.cache_control`, `usage.cache_creation_input_tokens`, `usage.cache_read_input_tokens`                                                    |
| 2024-09-24 | `message-batches-2024-09-24`               | Batch message processing            | A        | `requests[*].custom_id`, `requests[*].params`, `processing_status`, `request_counts.*`, `results_url`                                      |
| 2024-09-25 | `pdfs-2024-09-25`                          | PDF document support                | A,B,V,F  | `messages[*].content[*].source.media_type`, `messages[*].content[*].source.data`, `messages[*].content[*].source.type`                     |
| 2024-10-22 | `computer-use-2024-10-22`                  | Computer use tools (v1)             | A,B,V,F  | `tools[*].type:"computer_20241022"`, `tools[*].display_width_px`, `tools[*].display_height_px`, `tools[*].display_number`                  |
| 2024-11-01 | `token-counting-2024-11-01`                | Token counting endpoint             | A,B,V,F  | `input_tokens`, `cache_creation_input_tokens`, `cache_read_input_tokens`                                                                   |
| 2025-01-24 | `computer-use-2025-01-24`                  | Computer use tools (v2)             | A,B,V,F  | `tools[*].type:"computer_20250124"`, `tools[*].display_width_px`, `tools[*].display_height_px`                                             |
| 2025-02-19 | `token-efficient-tools-2025-02-19`         | Reduce tool definition tokens       | A,B,V,F  | `tools[*].defer_loading`, `tools[*].strict`                                                                                                |
| 2025-02-19 | `output-128k-2025-02-19`                   | Extend max output to 128k           | A,B,V,F  | `max_tokens` (up to 128000)                                                                                                                |
| 2025-04-04 | `mcp-client-2025-04-04`                    | MCP server integration (v1)         | A,B,V,F  | `mcp_servers[*].url`, `mcp_servers[*].tool_configuration`, `content[*].type:"mcp_tool_use"`, `content[*].type:"mcp_tool_result"`           |
| 2025-04-11 | `extended-cache-ttl-2025-04-11`            | Extended cache TTL to 1h            | A,B,V,F  | `*.cache_control.ttl:"1h"`                                                                                                                 |
| 2025-04-14 | `files-api-2025-04-14`                     | File upload/download API            | A,B,V,F  | `source.type:"file"`, `source.file_id`                                                                                                     |
| 2025-05-14 | `dev-full-thinking-2025-05-14`             | Full thinking content (dev)         | A,B,V,F  | `thinking.type:"enabled"`, `thinking.budget_tokens`, `content[*].type:"thinking"`                                                          |
| 2025-05-14 | `interleaved-thinking-2025-05-14`          | Thinking interleaved with tool use  | A,B,V,F  | `content[*].type:"thinking"` interleaved with `content[*].type:"tool_use"`                                                                 |
| 2025-05-22 | `code-execution-2025-05-22`                | Code execution sandbox              | A,B,V,F  | `tools[*].type:"code_execution_20250522"`, `tools[*].allowed_callers`, `content[*].caller`, `content[*].type:"code_execution_tool_result"` |
| 2025-06-27 | [context-management-2025-06-27]            | Auto context management             | A,B,V,F  | `context_management.edits[*]`, `context_management.edits[*].trigger`, `context_management.edits[*].keep`                                   |
| 2025-08-07 | [context-1m-2025-08-07]                    | 1M token context window             | A,B,V,F  | `max_tokens` (model context extended to 1M)                                                                                                |
| 2025-08-26 | [model-context-window-exceeded-2025-08-26] | Context window exceeded stop reason | A,B,V,F  | `stop_reason:"model_context_window_exceeded"`                                                                                              |
| 2025-10-02 | [skills-2025-10-02]                        | Skills/container support            | A,B,V,F  | `container.skills[*].id`, `container.skills[*].type`, `container.id`                                                                       |
| 2025-11-20 | [mcp-client-2025-11-20]                    | MCP server integration (v2)         | A,B,V,F  | `mcp_servers[*].url`, `mcp_servers[*].tool_configuration`                                                                                  |
| 2026-02-01 | [fast-mode-2026-02-01]                     | Fast inference mode                 | A        | `speed:"fast"`                                                                                                                             |

> A=Anthropic API, B=Bedrock, V=Vertex AI, F=Foundry

- prompt-caching-2024-07-31
  - 5m 缓存
  - 不再需要，默认启用，cache_control 控制
  - `cache_control: {type: "ephemeral"}`
- extended-cache-ttl-2025-04-11
  - 1h 缓存
  - `cache_control: {type: "ephemeral", ttl: "1h"}`

---

- https://platform.claude.com/docs/en/api/beta#anthropic_beta

[fast-mode-2026-02-01]: #fast-mode-2026-02-01

```
anthropic-beta: A,B
# SSE, 去掉 data: [DONE], named event
anthropic-version: 2023-06-01
# 第一个版本
anthropic-version: 2023-01-01
```

---

- https://platform.claude.com/docs/en/api/beta-headers

## output-128k-2025-02-19

允许输出 128K

## extended-cache-ttl-2025-04-11

- `messages[*].content[*].cache_control.ephemeral.ttl`

## code-execution-2025-05-22

```bash
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "content-type: application/json" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-05-22" \
  -d '{
      "model": "claude-sonnet-4-20250514",
      "max_tokens": 4096,
      "tools": [
        {
          "type": "code_execution_20250522",
          "name": "code_execution"
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "Calculate the first 20 Fibonacci numbers"
        }
      ]
    }'
```

```bash
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "content-type: application/json" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-05-22" \
  -d '{
      "model": "claude-sonnet-4-20250514",
      "max_tokens": 4096,
      "tools": [
        {
          "type": "code_execution_20250522",
          "name": "code_execution"
        },
        {
          "name": "get_stock_price",
          "description": "Get the current stock price for a given ticker symbol",
          "allowed_callers": ["direct", "code_execution_20250825"],
          "input_schema": {
            "type": "object",
            "properties": {
              "ticker": { "type": "string" }
            },
            "required": ["ticker"]
          }
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "Write Python code to fetch AAPL and GOOGL stock prices and calculate which one is more expensive"
        },
        {
          "role": "assistant",
          "content": [
            { "type": "text", "text": "I will write code to compare the stock prices." },
            {
              "type": "tool_use",
              "id": "toolu_code_01",
              "name": "code_execution",
              "input": { "code": "aapl = tool.get_stock_price(ticker='AAPL')" }
            },
            {
              "type": "tool_use",
              "id": "toolu_stock_01",
              "name": "get_stock_price",
              "input": { "ticker": "AAPL" },
              "caller": {
                "type": "code_execution_20250825",
                "tool_id": "toolu_code_01"
              }
            }
          ]
        },
        {
          "role": "user",
          "content": [
            {
              "type": "tool_result",
              "tool_use_id": "toolu_stock_01",
              "content": "{\"price\": 195.50, \"currency\": \"USD\"}"
            }
          ]
        }
      ]
    }'
```

```
用户请求
  ↓
模型发起 code_execution (toolu_code_01)
  ↓
sandbox 代码调用 get_stock_price → 产生 tool_use + caller
  ↓                                         ↑
  ↓                              caller.tool_id = "toolu_code_01"
  ↓                              caller.type = "code_execution_20250825"
  ↓
API 返回 stop_reason: "tool_use"，你处理 tool_result 回传
  ↓
模型拿到结果，sandbox 继续执行（或请求下一个工具）
  ↓
最终输出结果
```

- allowed_callers：在 tools 定义中，控制谁可以调用 → 请求侧
- caller：在 tool_use block 中，标识谁实际触发了调用 → 响应侧
- 如果去掉 "code_execution_20250825" 只保留 ["direct"]，sandbox 代码就无权调用 get_stock_price，也不会出现带 caller 的 tool_use

## context-management-2025-06-27

## mcp-client-2025-11-20

- mcp_toolset
- default_config

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 2048,
  "mcp_servers": [
    {
      "type": "url",
      "name": "database-server",
      "url": "https://mcp-db.example.com"
    }
  ],
  "tools": [
    {
      "type": "tool_search_tool_regex_20251119",
      "name": "tool_search_tool_regex"
    },
    {
      "type": "mcp_toolset",
      "mcp_server_name": "database-server",
      "default_config": {
        "defer_loading": true
      },
      "configs": {
        "search_events": {
          "defer_loading": false
        }
      }
    }
  ],
  "messages": [
    {
      "role": "user",
      "content": "What events are in my database?"
    }
  ]
}
```

## advanced-tool-use-2025-11-20

- Claude API, Microsoft Foundry, 所有模型
- 模型按需取搜索 tool
- 工具可以定义 defer_loading: true
  - 只保留 name, description
  - 本质是 SDK 去做 RAG 相关逻辑，然后自动提供相关的检索
  - 有点类似 SKILL 的概念
  - 渐进式披露
- API 返回 3-5 个 tool_reference
  - tool_search_tool_search_result
- server_tool_use
  - 表示使用服务端 tool search
- 也可以客户端实现 search_tools
- 实现上千工具的使用
  - 高效准确的工具搜索
  - 字符匹配、语义、分类
  - 可用于支持 mcp-client-2025-11-20
    - 服务端动态发现工具来使用

**请求**

```json
{
  "tools": [
    { "type": "tool_search_tool_regex_20251119", "name": "tool_search_tool_regex" },
    {
      "name": "github.createPullRequest",
      "description": "Create a pull request",
      "input_schema": {},
      "defer_loading": true
    }
  ]
}
```

**响应**

```json
{
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "I'll search for tools to help with the weather information."
    },
    {
      "type": "server_tool_use",
      "id": "srvtoolu_01ABC123",
      "name": "tool_search_tool_regex",
      "input": {
        "query": "weather"
      }
    },
    {
      "type": "tool_search_tool_result",
      "tool_use_id": "srvtoolu_01ABC123",
      "content": {
        "type": "tool_search_tool_search_result",
        "tool_references": [{ "type": "tool_reference", "tool_name": "get_weather" }]
      }
    },
    {
      "type": "text",
      "text": "I found a weather tool. Let me get the weather for San Francisco."
    },
    {
      "type": "tool_use",
      "id": "toolu_01XYZ789",
      "name": "get_weather",
      "input": { "location": "San Francisco", "unit": "fahrenheit" }
    }
  ],
  "stop_reason": "tool_use"
}
```

---

- https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool
  - tool_search_tool_regex_20251119
  - tool_search_tool_bm25_20251119
- https://github.com/BerriAI/litellm/pull/19841/changes
  - Translate advanced-tool-use to Bedrock-specific headers for Claude Opus 4.5
  - advanced-tool-use-2025-11-20 -> tool-search-tool-2025-10-19 + tool-examples-2025-10-29
- https://www.anthropic.com/engineering/advanced-tool-use
- https://github.com/anthropics/claude-cookbooks/blob/main/tool_use/tool_search_with_embeddings.ipynb

## tool-examples-2025-10-29

- Opus 4.5+
- Vertex AI, Amazon Bedrock
- input_examples 字段

## fast-mode-2026-02-01

- 支持情况
  - Claude Opus 4.6
  - 2.5x 速度
  - 6x 价格
- fast: boolean

```bash
curl https://api.anthropic.com/v1/messages \
  --header "x-api-key: $ANTHROPIC_API_KEY" \
  --header "anthropic-version: 2023-06-01" \
  --header "anthropic-beta: fast-mode-2026-02-01" \
  --header "content-type: application/json" \
  --data '{
        "model": "claude-opus-4-6",
        "max_tokens": 4096,
        "speed": "fast",
        "messages": [{
            "role": "user",
            "content": "Refactor this module to use dependency injection"
        }]
    }'
```

---

- https://platform.claude.com/docs/en/build-with-claude/fast-mode

## US-Only Inference

- 1.1x 价格
- inference_geo: us

## effort

- `output: { effort: 'high' }`
- 支持 Opus 4.5, Opus 4.6
- max, high, medium, low
- 默认 high
- Opus 4.6+ 替代以前的 budget_tokens

## adaptive thinking

- Opus 4.6+
- thinking.type=adaptive
- 弃用 `thinking: {type: "enabled", budget_tokens: N}`

```bash
curl https://api.anthropic.com/v1/messages \
  --header "x-api-key: $ANTHROPIC_API_KEY" \
  --header "anthropic-version: 2023-06-01" \
  --header "content-type: application/json" \
  --data \
  '{
    "model": "claude-opus-4-6",
    "max_tokens": 16000,
    "thinking": {
        "type": "adaptive"
    },
    "messages": [
        {
            "role": "user",
            "content": "Explain why the sum of two even numbers is always even."
        }
    ]
}'
```

```
adaptive thinking is not supported on this model
```

## Tool Use (Anthropic vs AWS Bedrock)

### 1. `tool_choice` 参数格式差异

- **Anthropic 原生 API**：在根级别使用 `tool_choice` 对象配置，包含 `type: "auto" | "any" | "tool"`，开启时还需要 `name` 字段（如果 type 是 `tool`）。
  - 最近还引入了 `tool_choice: {"type": "none"}` 来彻底禁止走工具路径。
- **AWS Bedrock Converse API**：参数名包裹在更加庞大的 `toolConfig` 对象内。它的名字变成了 `toolConfig.toolChoice`。
  - `toolChoice` 可选项对应也是 `auto` / `any` / `tool`。
  - 注意：Bedrock 历史上长期不支持 `none`，并且在同时开启 Thinking 模型模式（需要更多的上下文自由度）强行指定 `tool` 时，曾爆出 ValidationException（原生 API 目前在开启 Thinking 时仅支持 auto 甚至会忽略 specific tool choice）。

### 2. `tool_reference` (Tool Search 特性)

> 参考 Beta 特性 `advanced-tool-use-2025-11-20` (也即后期的 Tool Search 集成等)

你提到的 `tool_reference` 是 Anthropic 官方新推的 **服务器端工具搜索 (Server-side Tool Search) / 延迟加载工具 (Deferred Loading)** 特型。

- **工作原理**：当通过 API 传入上百数千个工具时，为了节约 token 并不全量载入，而是先将工具设置为 `defer_loading: true`。模型在思考时，第一步会去内部触发搜索这些工具，然后作为辅助的中间件向内容块（Content block）中吐出 `type: "tool_reference"` 块（包含了指代待用工具的索引，随后被系统填充转化为完整的 `tool_use` 供后续进行下层调用）。
- **兼容性重灾区 (Bedrock 上的缺失)**：
  - AWS Bedrock 的 **Converse API 不原生支持** 响应体里解析或发出这种特有的 `tool_reference` block。
  - 从开源的 `claude-code` 与 Bedrock 的兼容性反馈 issue 中能看见：如果在 Bedrock 开启这个高级工具特性跑 Claude，当模型返回 `tool_reference` 时，由于 Bedrock Converse API 严格的 JSON Schema 白名单限制校验，这个 block 会直接引发 API 级别的拒绝错误。
  - 虽然 Bedrock 的低级 `InvokeModel` API 可能通过透传绕过一些验证，但在标准 Converse API 工作流下，原生高级工具搜索（Tool Reference）是不被支持的。
