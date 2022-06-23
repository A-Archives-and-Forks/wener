---
title: zsh
---

# zsh

- 配置
  - .zshrc - 交互 shell
  - .zprofile - 登陆 shell
- 参考
  - [robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
  - [zsh-users/antigen](https://github.com/zsh-users/antigen)

<!--  ✅❌🟡-->

| feature                                        | zsh | bash |
| ---------------------------------------------- | --- | ---- |
| Automatic cd                                   |
| Recursive path expansion                       |
| Spelling correction and approximate completion |
| Plugin and theme suppor                        |

# FAQ

## Why ZSH

- Pros
  - macOS 默认 Shell
- Cons
  - 大多 ZSH 有的特性 Bash 也有
    - 但 zsh 可能支持的更完善
  - 大多服务器环境都是 Bash 或 POSIX Shell
    - 平时使用 bash 更利于服务端编码
    - 过多使用 zsh 相关特性会产生依赖
