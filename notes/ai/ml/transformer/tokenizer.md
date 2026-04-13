---
title: Tokenizer
---

# Tokenizer

:::tip Workflow

- Tokenization/分词/Encode - 文本 -> Token IDs
  - 一般使用 BPE 算法
- Decode - Token IDs -> 文本

:::

- 算法
  - BPE - Byte Pair Encoding - 基于频率合并、自底向上 - BP 数据压缩算法
    - Byte Level
      - 能处理任意数据，不会出现 `[UNK]`
    - Character Level
    - Word Level
    - e.g. gpt2, llama
  - WordPiece - 基于概率合并、自底向上
    - e.g. bert
    - https://arxiv.org/pdf/1609.08144.pdf
  - Unigram - 基于概率剪枝、自顶向下
    - e.g. T5, ALBERT
- digram coding / Byte-pair encoding / [BPE](https://en.wikipedia.org/wiki/Byte_pair_encoding) tokeniser
- [SentencePiece](https://github.com/google/sentencepiece)
  - Apache-2.0, C++
  - 支持 BPE, unigram, char, word
- [openai/tiktoken](https://github.com/openai/tiktoken)
  - pypi tiktoken
  - 定义的模型 https://github.com/openai/tiktoken/blob/main/tiktoken_ext/openai_public.py
- [huggingface/tokenizers](https://github.com/huggingface/tokenizers)
  - Apache-2.0, Rust
  - Go binding [daulet/tokenizers](https://github.com/daulet/tokenizers)
  - Proposal: Add Golang Bindings for tokenizers [huggingface/tokenizers#1751](https://github.com/huggingface/tokenizers/issues/1751)
- https://github.com/rsennrich/subword-nmt
  - Unsupervised Word Segmentation for Neural Machine Translation and Text Generation
  - Subword Neural Machine Translation
- [dqbd/tiktoken](https://github.com/dqbd/tiktoken)
  - JS port and JS/WASM bindings for openai/tiktoken
  - https://github.com/dqbd/tiktoken/blob/main/tiktoken/model_to_encoding.json
  - https://github.com/dqbd/tiktoken/blob/main/tiktoken/registry.json
  - Online https://tiktokenizer.vercel.app/
    - [dqbd/tiktokenizer](https://github.com/dqbd/tiktokenizer)
      - Online playground
  - https://tiktokenizer.vercel.app/hf/Qwen/Qwen2.5-72B/tokenizer.json
    - https://huggingface.co/Qwen/Qwen2.5-72B/blob/main/tokenizer.json
- [zurawiki/tiktoken-rs](https://github.com/zurawiki/tiktoken-rs)
  - MIT, Rust
  - https://crates.io/crates/tiktoken-rs
- Typescript/JS/Wasm
  - [niieani/gpt-tokenizer](https://github.com/niieani/gpt-tokenizer)
  - https://github.com/anthropics/anthropic-tokenizer-typescript
    - < Claude 3
- Golang
  - [tiktoken-go/tokenizer](https://github.com/tiktoken-go/tokenizer)
    - MIT, Go
  - [pkoukk/tiktoken-go](https://github.com/pkoukk/tiktoken-go)
    - MIT, Go
- https://platform.openai.com/tokenizer

**ChatGPT 特殊 Token**

```
<|endoftext|>
<|endofprompt|>
<|eos|>
<|pad|>
<|bos|>
<|eol|>
<|math|>
<|doc|>

<|im_start|>
<|im_end|>
<|im_sep|>

<|fim_prefix|>
<|fim_middle|>
<|fim_suffix|>
```

- add_special_tokens
  - 序列标记
  - 对话标记
  - 功能标记

---

| model                | vocab size | tokenizer      | notes |
| -------------------- | ---------- | -------------- | ----- |
| openai/gpt2          | 50257      | byte-level-bpe |
| openai/r50k_base     | 50257      | byte-level-bpe |
| openai/p50k_base     | 50281      | byte-level-bpe |
| openai/p50k_edit     | 50283      | byte-level-bpe |
| openai/cl100k_base   | 100276     | byte-level-bpe |
| openai/o200k_base    | 200018     | byte-level-bpe |
| openai/o200k_harmony | 201088     | byte-level-bpe |

- o200k_harmony
  - vocabe 201088
  - 在 c200k_base 上增加了额外的特殊 token

| abbr.   | stand for                     | meaning |
| ------- | ----------------------------- | ------- |
| BPE     | Byte Pair Encoding            |
| unigram | Unigram                       |
| NFKC    | Unicode Normalization Form KC |

## SentencePiece

- spm_encode
- spm_decode
- spm_normalize
- spm_train
- spm_export_vocab

```bash
pip install sentencepiece
```

## Multi Model

- Gemini
  - 1 token ≈ 4 字符
  - 100 tokens ≈ 60-80 单词
  - **多模态 (Multimodal)**:
    - **音频 (Audio)**: 固定速率 **32 tokens/s**
    - **视频 (Video)**: 固定速率 **263 tokens/s**
    - **图像 (Image)**:
      - 边长均 ≤ 384px: 消耗 **258 tokens**
      - 超过 384px: 图像会被切分为 **768x768** 的网格分片 (Tiles)，每个分片消耗 **258 tokens**
- Qwen2.5-VL
  - **动态分辨率**: 不强制缩放到固定正方形，保留原始比例
  - **计算公式**: $\text{Tokens} = \lceil H/28 \rceil \times \lceil W/28 \rceil + 2$
    - 底层 Patch 为 14x14，经过 2x2 合并（Pooling）后，每个 Token 对应 **28x28像素** 的区域
    - 额外 +2 是因为包含 `<|vision_start|>` 和 `<|vision_end|>`
- Qwen 3.5
  - **原生多模态 (Native Multimodal)**: 采用早期融合（Early Fusion）架构，文本与视觉 Token 在同一空间处理。
  - **Token 消耗**:
    - 依然遵循 **动态分辨率** 逻辑，通常按 **28x28 像素/Token** 比例转换（继承 2.5-VL 的高效表征）。
    - 视频处理通过时间轴切片，每秒视频转换的 Token 数与帧率及分辨率挂钩。

---

- https://ai.google.dev/gemini-api/docs/tokens

# FAQ

## tokenizer.json vs vocab.json

- vocab.json + merges.txt

```py
tokenizer = Tokenizer.from_file("byte-level-bpe.tokenizer.json)
# 会保存为 merges.txt+vocab.json
tokenizer.model.save('tokenizer')
```
