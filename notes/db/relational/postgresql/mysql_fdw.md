---
tags:
  - MySQL
  - FDW
  - Extensions
---

# mysql_fdw

- [EnterpriseDB/mysql_fdw](https://github.com/EnterpriseDB/mysql_fdw)
- ⚠️ 需要 prepare stmt
  - mysql_stmt_prepare
  - 因此有些 connector 场景不支持

```sql
CREATE EXTENSION mysql_fdw;

CREATE SERVER mysql_server
FOREIGN DATA WRAPPER mysql_fdw
OPTIONS (host '127.0.0.1', port '3306');

CREATE USER MAPPING FOR postgres
SERVER mysql_server
OPTIONS (username 'foo', password 'bar');

CREATE FOREIGN TABLE warehouse(
     warehouse_id int,
     warehouse_name text,
     warehouse_created datetime)
SERVER mysql_server
     OPTIONS (dbname 'db', table_name 'warehouse');
```
