---
title: pgsodium
---

# pgsodium

- [michelp/pgsodium](https://github.com/michelp/pgsodium)
  - PostgreSQL License, C, libsodium, PostgreSQL
  - pgsodium 是一个 PostgreSQL 扩展，通过封装 libsodium 库提供加密功能。它在 Supabase 中被广泛用于提供透明列表加密（TCE）和服务器主密钥管理。

- 参考
  - [pgsodium Documentation](https://pgsodium.org/)
  - [Supabase Cryptography Tools](https://supabase.com/docs/guides/database/extensions/pgsodium)

# 核心特性

1. **服务器密钥管理 (Server Key Management)**: 启动时加载根密钥，永远不会通过 SQL 泄露。通过 key ID 和上下文派生子密钥。
2. **透明列表加密 (Transparent Column Encryption, TCE)**: 通过 `SECURITY LABEL`、触发器和视图实现自动加解密。应用层读写明文，扩展层处理加密。
3. **直接 libsodium API**: 提供了超过 190 个 SQL 函数，直接映射 libsodium 的加密原语（如 Authenticated Encryption、数字签名、哈希等）。

# 使用示例

```sql
-- 安装扩展
CREATE EXTENSION pgsodium;

-- 生成随机数
SELECT pgsodium.randombytes_random();

-- 生成密钥（32 字节）
SELECT pgsodium.crypto_secretbox_keygen() AS key \gset

-- 加密消息
SELECT pgsodium.crypto_secretbox('hello world', pgsodium.randombytes_buf(pgsodium.crypto_secretbox_noncebytes()), :'key');

-- 使用派生密钥加密 (推荐方式，不需要在 SQL 中传递原始密钥)
SELECT pgsodium.crypto_secretbox_by_id(
    'secret message',
    pgsodium.crypto_secretbox_noncegen(),
    1,              -- key_id
    'pgsodium'      -- context (8 字节)
);
```

# FAQ

- **如何启用 TCE？**
  - 使用 `SECURITY LABEL FOR pgsodium ON COLUMN ... IS 'ENCRYPT WITH KEY ID ...';`
- **对 PostgreSQL 版本有要求吗？**
  - 需要 PostgreSQL 14 或更高版本。
