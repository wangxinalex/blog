---
title: "My First Post"
date: 2019-09-15T18:12:58+08:00
draft: false
---

# 一些常用的zsh插件

## 安装zsh

```bash
brew install zsh
```

## 安装oh-my-zsh
https://github.com/robbyrussell/oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## zsh插件

### auto-suggestions

https://github.com/zsh-users/zsh-autosuggestions

1. Clone this repository into $ZSH_CUSTOM/plugins (by default ~/.oh-my-zsh/custom/plugins)
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
2. Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):
```bash
plugins=(zsh-autosuggestions)
```
3. Start a new terminal session.

### zsh-syntax-highlighting

https://github.com/zsh-users/zsh-syntax-highlighting

1. Clone this repository in oh-my-zsh's plugins directory:
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
2. Activate the plugin in ~/.zshrc:
```bash
plugins=( [plugins...] zsh-syntax-highlighting)
```
3. Restart zsh (such as by opening a new instance of your terminal emulator).

### zsh-history-substring-search

https://github.com/zsh-users/zsh-history-substring-search

1. Clone this repository in oh-my-zsh's plugins directory:
```bash
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```
2. Activate the plugin in ~/.zshrc:
```bash
plugins=( [plugins...] history-substring-search)
```
3. Source ~/.zshrc to take changes into account:
```bash
source ~/.zshrc
```
4. Bind keyboard shortcuts to this script's functions.
```bash
bindkey '^[[A' history-substring-search-up
bindkey '^[[B' history-substring-search-down
```

### z

just add `z` to the `plugins` array in `.zshrc`

```bash
plugins=( [plugins...] z )
```