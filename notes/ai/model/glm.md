---
title: GLM
---

# GLM

- [zai-org/GLM-4.1V-Thinking](https://github.com/zai-org/GLM-4.1V-Thinking)
  - 9B, 64K context, Vision, Reasoning
  - 基于 GLM-4-9B-0414
  - glmv_reward
- `<|begin_of_box|>`, `<|end_of_box|>`
  - 核心答案
  - 可能会导致格式错误
  - https://github.com/zai-org/GLM-4.1V-Thinking/issues/79
- `<think>`
- `</think>`
- `<answer>`
- `</answer>`

## glm-ocr

```yaml
default_prompt: >
  Recognize the text in the image and output in Markdown format.
  Preserve the original layout (headings/paragraphs/tables/formulas).
  Do not fabricate content that does not exist in the image.
task_prompt_mapping:
  text: 'Text Recognition:'
  table: 'Table Recognition:'
  formula: 'Formula Recognition:'
```

信息提取

```
请按下列JSON格式输出图中信息:
{
    "id_number": "",
    "last_name": "",
    "first_name": "",
    "date_of_birth": "",
    "address": {
        "street": "",
        "city": "",
        "state": "",
        "zip_code": ""
    },
    "dates": {
        "issue_date": "",
        "expiration_date": ""
    },
    "sex": ""
}
```
