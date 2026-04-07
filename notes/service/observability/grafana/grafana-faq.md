---
tags:
  - FAQ
---

# Grafana FAQ

## SchemaVersion

- https://github.com/grafana/grafana/blob/main/public/app/features/dashboard/state/DashboardMigrator.ts

| schemaVersion | 关键变更                                        | 对应 Grafana 版本 |
| ------------- | ----------------------------------------------- | ----------------- |
| 2-8           | 早期 filter/timepicker/InfluxDB 迁移            | 很早期            |
| 9-13          | singlestat→stat, graph→timeseries 等面板迁移    | ~v5-v6            |
| 16            | Grid layout 布局迁移                            | ~v5               |
| 24            | Angular table→table-old                         | v7.0              |
| 27            | 移除 repeated panels, constant 变量转换         | ~v7.x             |
| 30            | value mappings 升级                             | ~v8.x             |
| 33            | datasource name → uid/type object 引用          | v8.3              |
| 36            | annotations/variables 中 datasource 引用迁移    | ~v9.x             |
| 37-39         | legend, table cellOptions, timeseries transform | ~v10.x            |
| 40-41         | refresh 属性、timepicker cleanup                | ~v11.x            |
| 42            | hideFrom.viz 迁移，标记为 v1 API 最终版本       | v12.0+            |

## Public

- 公开访问
