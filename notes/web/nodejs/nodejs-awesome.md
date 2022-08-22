---
title: Nodejs Awesome
tags:
  - Awesome
---

# Nodejs Awesome

:::tip

- 最好选择 TypeScript 开发的或支持 TypeScript 的
- TypeScript 的 decorator 比 Java 的 Annotation 弱得多
  - 不支持获取字段实际类型信息 - 因为不存在这样的信息

:::

:::caution Nodejs 后端开发不太活跃

最近一两年 (2020-2021)，可能是因为 Go 和 Rust 的盛行，导致 Nodejs 的后端开发弱化了，很多项目开发都不太活跃。

:::

## DB

| driver pkg     | db                   |
| -------------- | -------------------- |
| pg pg-hstore   | PostgreSQL           |
| mysql2         | MySQL                |
| mariadb        | MariaDB              |
| sqlite3        | SQLite               |
| better-sqlite3 | SQLite               |
| tedious        | Microsoft SQL Server |
| ibm_db         | DB2                  |

- better-sqlite3
  - 支持自定义函数
  - 支持迭代 cursor
  - 支持 int64
  - [Convince me to use better-sqlite3](https://github.com/WiseLibs/better-sqlite3/issues/262)
- [sequelize/sequelize](./sequelize.md)
  - ORM
  - Postgres, MySQL, MariaDB, SQLite, Microsoft SQL Server.
  - 因为需要支持很多 DB 类型，丢失一定的特性
  - Use better-sqlite3 [#11400](https://github.com/sequelize/sequelize/issues/11400)
- [knex/knex](https://github.com/knex/knex)
  - SQL Builder
  - Composite - 解耦构建最终 query 的过程
  - 对于基础的访问模式提供跨库支持
  - ⚠️ 不支持 typescript 实体类型安全
  - Postgres, MSSQL, MySQL, MariaDB, SQLite3, Oracle, Amazon Redshift
  - 🚧 开发缓慢
- prisma
- [gajus/slonik](https://github.com/gajus/slonik)
  - PostgreSQL client with strict types
  - string tag
- [mikro-orm/mikro-orm](https://github.com/mikro-orm/mikro-orm)
  - 基于 knex
- [koskimas/kysely](https://github.com/koskimas/kysely)
  - type-safe typescript SQL query builder
  - [RobinBlomberg/kysely-codegen](https://github.com/RobinBlomberg/kysely-codegen)
    - schema -> ts
- [Vincit/objection.js](https://github.com/Vincit/objection.js)
  - 🚧 开发停滞
  - SQL-friendly ORM
  - 基于 knex
- [typeorm/typeorm](https://github.com/typeorm/typeorm)
  - 基于 typescript decoration 的 ORM
  - 🚧 开发缓慢
- [bookshelf/bookshelf](https://github.com/bookshelf/bookshelf)
  - 基于 knex 的 ORM
  - 🚧 开发停止
- [balderdashy/waterline](https://github.com/balderdashy/waterline)
  - 🚧 开发停止 - 2021
- [dmfay/massive-js](https://gitlab.com/dmfay/massive-js)
  - data mapper for Node.js and PostgreSQL

## Library

- [timgit/pg-boss](https://github.com/timgit/pg-boss)
  - Node.js using PostgreSQL like a boss
- [breejs/bree](https://github.com/breejs/bree)
  - job scheduler
- WebSocket
  - [ws](https://github.com/websockets/ws)
    - JS 实现
    - isomorphic-ws 可用于 web
  - [uWebSockets.js](https://github.com/uNetworking/uWebSockets.js)
    - 基于 [uWebSockets](https://github.com/uNetworking/uWebSockets) C++ 库
      - 基于 [uNetworking/uSockets](https://github.com/uNetworking/uSockets)
    - 性能好
    - 针对 Linux 优化

## Web

- [fastify](./fastify.md)
  - 🌟 推荐
- express
- koa
  - 🚧 开发停滞

## Server

- 日志
  - [pinojs/pino](https://github.com/pinojs/pino)

## Scraper

- [jsdom/jsdom](https://github.com/jsdom/jsdom)
- [cheeriojs/cheerio](https://github.com/cheeriojs/cheerio)
- [rchipka/node-osmosis](https://github.com/rchipka/node-osmosis)
- https://www.webscrapingapi.com/

## Browser Automation

- [puppeteer/puppeteer](https://github.com/puppeteer/puppeteer)
- [Microsoft/playwright](https://github.com/Microsoft/playwright)
  - 统一 API 支持 Chromium, Firefox, WebKit
- [matthewmueller/x-ray](https://github.com/matthewmueller/x-ray)
  - web scraper
- [segmentio/nightmare](https://github.com/segmentio/nightmare)
  - 基于 Electron

## 工具

- shell
  - [google/zx](https://github.com/google/zx)
