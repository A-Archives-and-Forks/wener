---
title: kimi
---

# kimi

# FAQ

## Typescript namespace

tool 有些使用的类似 ts 的 namespace

```ts
namespace functions {
  // Read the contents of a file. Supports text files and images...
  type read = (_: {
    // Path to the file to read (relative or absolute)
    file_path?: string;
    // Maximum number of lines to read
    limit?: number;
    // Line number to start reading from (1-indexed)
    offset?: number;
  }) => any;
}
```
