---
tags:
  - Reference
---

# Grafana Reference

| Macro                 | Description                                |
| --------------------- | ------------------------------------------ |
| `${__org.id}`         | grafana org id where the request came from |
| `${__plugin.id}`      | the plugin id                              |
| `${__plugin.version}` | the plugin version                         |
| `${__ds.uid}`         | the datasource uid                         |
| `${__ds.name}`        | the datasource name                        |
| `${__ds.id}`          | the datasource id (deprecated)             |
| `${__user.login}`     | the user login id                          |
| `${__user.email}`     | the user login email                       |
| `${__user.name}`      | the user name                              |

```
${__from:date:YYYY-MM}
${__to:date:YYYY-MM}

${__timeFrom}
${__timeTo}
```

## annotations

```sql
SELECT
  created_at,
  action,
  operator,
  id,
  target,
  title ,
  text
FROM audit_logs
WHERE $__timeFilter(created_at)
-- WHERE created_at >= FROM_UNIXTIME($__unixEpochFrom()) AND created_at <= FROM_UNIXTIME($__unixEpochTo())
ORDER BY id DESC
LIMIT 50;
```

- `$__timeFilter(created_at)`
  - -> `created_at BETWEEN '2023-11-28 10:00:00' AND '2023-11-28 11:00:00'`
- Quota 问题
  - multi value 的时候
  - `${var:raw}` 得到原始，然后自己做 quote 处理

## Variable Formatting Options

语法：`${var_name:format}`

| 格式             | 输出          | 转义 & 结合逻辑                                         | 常见用途                  |
| :--------------- | :------------ | :------------------------------------------------------ | :------------------------ |
| `:regex`         | `(a\.b\|c)`   | **转义** 正则特殊字符（`.` -> `\.`），外层加 **括号**。 | `{label=~"${var:regex}"}` |
| `:pipe`          | `a.b\|c`      | **不转义** 特殊字符，仅用 `\|` 分隔，**无括号**。       | 自定义正则组合            |
| `:raw`           | `a.b,c`       | **不转义**，多选时以 **逗号** 分隔。                    | 别名、防止二次转义        |
| `:csv`           | `a.b,c`       | 同 `:raw`，逗号分隔。                                   | SQL `IN` 操作 (非字符串)  |
| `:json`          | `["a.b","c"]` | 转换为 JSON 数组格式。                                  | API 请求体                |
| `:percentencode` | `a.b%2Cc`     | URL 编码（主要针对分隔符）。                            | URL 参数                  |
| `:singlequote`   | `'a.b','c'`   | 单引号包裹，逗号分隔。                                  | SQL 字符串列表            |
| `:doublequote`   | `"a.b","c"`   | 双引号包裹，逗号分隔。                                  | JSON 或某些 SQL           |
| `:sqlstring`     | `'a.b','c'`   | 单引号包裹，并 **转义单引号** (`'` -> `''`)。           | 安全的 SQL 注入防护       |
| `:glob`          | `{a.b,c}`     | Glob 格式。                                             | Graphite 查询             |
