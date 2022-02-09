---
description: oh-my-zsh 配置 自用
---

# oh-my-zsh

## 插件

- zsh-autojump
- zsh-syntax-highlighting
- zsh-autosuggestions

  ```zsh
  bindkey '^I' autosuggest-accept
  ```

## Git

### diff-so-fancy

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

## 主题

### [Spaceship](https://spaceship-prompt.sh/getting-started/)

```zsh
ZSH_THEME="spaceship"
SPACESHIP_PACKAGE_SHOW=false
SPACESHIP_DOCKER_SHOW=false
SPACESHIP_NODE_SHOW=false
SPACESHIP_RUBY_SHOW=false
SPACESHIP_DIR_TRUNC_PREFIX=…/
```
