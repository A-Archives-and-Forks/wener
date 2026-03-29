---
title: Vercel AI SDK
---

# AI SDK

- [vercel/ai](https://github.com/vercel/ai)
  - Apache-2.0, TS
- 参考
  - 如何设计持久化存储 https://github.com/vercel/ai/discussions/4845
  - https://github.com/midday-ai/ai-sdk-tools
  - 很多生态都支持 AI SDK Language Model: volagent, mantra, opencode

## Notes

- Provider 模型抽象
- LanguageModel
  - doGenerate
  - doStream
- EmbeddingsModel
  - doEmbed
- ImageModel
  - doGenerate
- TranscriptionModel
  - doTranscribe
- SpeechModel
  - doGenerate
- RerankingModel
  - doRerank

## Design

| finish reason    | openai finish reason          | anthropic stop reason                                 | gemini finish reason                           |
| ---------------- | ----------------------------- | ----------------------------------------------------- | ---------------------------------------------- |
| `stop`           | `stop`                        | `pause_turn`, `end_turn`, `stop_sequence`, `tool_use` | `STOP`(无工具调用)                             |
| `length`         | `length`                      | `max_tokens`, `model_context_window_exceeded`         | `MAX_TOKENS`                                   |
| `content-filter` | `content_filter`              | `refusal`                                             | `IMAGE_SAFETY`, `RECITATION`, `SAFETY`, `BLOCKLIST`, `PROHIBITED_CONTENT`, `SPII` |
| `tool-calls`     | `function_call`, `tool_calls` | `tool_use`                                            | `STOP`(带工具调用)                             |
| `error`          | -                             | -                                                     | `MALFORMED_FUNCTION_CALL`                      |
| `other`          | `other`                       | `compaction`, `other`                                 | `FINISH_REASON_UNSPECIFIED`, `OTHER`           |

| tool choice | openai                                     | anthropic                           | gemini                                                              |
| ----------- | ------------------------------------------ | ----------------------------------- | ------------------------------------------------------------------- |
| `auto`      | `'auto'`                                   | `{ type: 'auto' }`                  | `mode: 'AUTO'` / `'VALIDATED'`                                      |
| `none`      | `'none'`                                   | 不支持(SDK 层面移除所有 tools 模拟) | `mode: 'NONE'`                                                      |
| `required`  | `'required'`                               | `{ type: 'any' }`                   | `mode: 'ANY'` / `'VALIDATED'`                                       |
| `tool`      | `{ type: 'function', function: { name } }` | `{ type: 'tool', name }`            | `mode: 'ANY'` (包含 `allowedFunctionNames: [name]`) / `'VALIDATED'` |
