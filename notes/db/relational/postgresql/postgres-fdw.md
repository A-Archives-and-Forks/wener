---
title: FDW
---

# FDW

| abbr. | stand for            | cn  |
| ----- | -------------------- | --- |
| FDW   | Foreign Data Wrapper | 外部数据包装器
| FT    | Foreign Table        | 外部表   |

- handler
  - FdwRoutine
    - GetForeignRelSize
    - GetForeignPaths
    - GetForeignPlan
    - GetForeignModifyPath
    - BeginForeignScan
    - EndForeignScan
    - ReScanForeignScan
- validator
  - OPTIONS

```bash
SELECT *
FROM information_schema.foreign_data_wrappers

SELECT fdwname, fdwhandler, fdwvalidator
FROM pg_foreign_data_wrapper
```

- wrappers
- https://github.com/wenerme/wrappers
