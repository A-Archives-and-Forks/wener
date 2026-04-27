---
title: ghostty
---

# ghostty

- [ghostty-org/ghostty](https://github.com/ghostty-org/ghostty)
  - MIT, Zig, Swift
  - 支持 Tmux Control Mode

:::caution

- Support for tmux's Control Mode https://github.com/ghostty-org/ghostty/issues/1935

:::

```bash
brew install ghostty
# apk add ncurses # AlpineLinux for tic
infocmp -x | ssh SERVER -- tic -x -

ghostty +show-config

ghostty +ssh-cache --remove=user@host

infocmp xterm-ghostty
echo $TERM

# Command+Shift+, 可以 reload 配置
# 修改配置
EDITOR=zed ghostty +edit-config
ghostty +show-config

ghostty +list-fonts
ghostty +show-face --string="A"
ghostty +show-face --string="你好"
```
