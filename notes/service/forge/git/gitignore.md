---
title: .gitignore
---

# .gitignore

- https://github.com/github/gitignore
- https://www.toptal.com/developers/gitignore
- https://gitignore.itranswarp.com/
- https://git-scm.com/docs/gitignore
- 语法
  - `*`
  - `?`

```
# comment

# 绝对匹配
secrets.txt

logs/
*.log  # Ignores all files with a .log extension
!important.log
file?.txt # Matches file1.txt, fileA.txt, etc.
log[0-9].txt # Matches log1.txt, log2.txt, etc.
debug[!01].log
**/logs/
/config.json # Only ignores config.json in the root directory
```

**local**

```gitignore
### Base
.idea

### OS
Thumbs.db
ehthumbs.db
Desktop.ini
.DS_Store
._*
.fseventsd

### Temporary
*.tgz
*.zip
*.tar
*.log
node_modules/
.wrangler/cache/

### Local files
local.mk
*.local.mk
*.local.json
*.local.md
*.local.http
*.local.yaml
*.local.sql
*.local.test.ts
local.test.ts
*.local.sh
*.local.py
*.local.csv
local.just
*.local.just
*.local.txt
local/

### Secrets
*.private.env.json
*.password.txt
htpasswd
kubeconfig.yaml
*.kubeconfig.yaml
*.priv
*.crt
*.key
.env
.env.*
*.env

### AI
.agents/
.claude/
.codex/
.cursor/
.gemini/
.opencode/
CLAUDE.md
AGENTS.md
```
