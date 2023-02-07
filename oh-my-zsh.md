---
description: oh-my-zsh 配置 自用
---

# Mackup

找到了一个好工具 [Mackup](https://github.com/lra/mackup) 🚀，可以用来同步自己各种应用的配置，所以 `dotFiles` 都可以同步到 [Github 仓库](https://github.com/zo-ly/mackup_cfg) 啦 🎉

# ~~oh-my-zsh~~

## ~~插件~~

- ~~zsh-autojump~~
- ~~zsh-syntax-highlighting~~
- ~~zsh-autosuggestions~~

  ```zsh
  bindkey '^I' autosuggest-accept
  ```

## ~~Git~~

### ~~diff-so-fancy~~

```yaml
[alias]
  co = checkout
  s = status
[pull]
  ff = only
[core]
  editor = nvim
  pager = diff-so-fancy | less --tabs=4 -RFX
[interactive]
  diffFilter = diff-so-fancy --patch
[color]
  ui = true
[color "diff-highlight"]
  oldNormal = red bold
  oldHighlight = red bold 52
  newNormal = green bold
  newHighlight = green bold 22
[color "diff"]
  meta = 11
  frag = magenta bold
  func = 146 bold
  commit = yellow bold
  old = red bold
  new = green bold
  whitespace = red reverse
```

## ~~主题~~

### ~~[Spaceship](https://spaceship-prompt.sh/getting-started/)~~

```zsh
ZSH_THEME="spaceship"
SPACESHIP_PACKAGE_SHOW=false
SPACESHIP_DOCKER_SHOW=false
SPACESHIP_NODE_SHOW=false
SPACESHIP_RUBY_SHOW=false
SPACESHIP_DIR_TRUNC_PREFIX=…/
```
