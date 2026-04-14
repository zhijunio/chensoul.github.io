---
title: "macOS 开发环境配置清单"
date: 2026-04-14 18:00:00+08:00
slug: macos-dev-environment-checklist
categories: [ "tech" ]
tags: [ "macos", "tools", "checklist" ]
draft: false
description: "macOS 开发环境配置完整清单：Homebrew、Shell 环境、编辑器、效率工具和开发工具。"
favicon: "macos.svg"
---

> 基于 macOS 14+ (ARM64) 环境，更新于 2026-04-14

## 一、包管理器

### Homebrew

```bash
# 安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 配置镜像源（可选，中国大陆用户）
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
```

### Brewfile 核心工具

```ruby
# 开发工具
brew "git"
brew "gh"                    # GitHub CLI
brew "fnm"                   # Node.js 版本管理
brew "uv"                    # Python 包管理
brew "sdkman/tap/sdkman-cli" # Java 生态管理

# 现代 CLI 工具
brew "bat"                   # cat 替代
brew "eza"                   # ls 替代
brew "fd"                    # find 替代
brew "ripgrep"               # grep 替代
brew "fzf"                   # 模糊搜索
brew "jq"                    # JSON 处理
brew "yq"                    # YAML 处理
brew "duckdb"                # 轻量 SQL
brew "xh"                    # HTTP 客户端
brew "git-delta"             # diff 增强
brew "just"                  # 命令运行器
brew "watchexec"             # 文件监听执行
brew "sd"                    # 文本替换
brew "zoxide"                # cd 增强

# 终端工具
brew "starship"              # 提示符主题
brew "ghostty"               # 终端模拟器
brew "chezmoi"               # dotfiles 管理

# 图形应用
cask "cursor"                # AI 编辑器
cask "claude-code"           # Claude Code CLI
cask "codex"                 # Codex CLI
cask "ghostty"               # 终端
cask "orbstack"              # Docker 替代
cask "switchhosts"           # hosts 管理
cask "tableplus"             # 数据库客户端
```

安装：`brew bundle install`

## 二、Shell 环境

### zsh + Sheldon

使用 **Sheldon** 替代 oh-my-zsh 管理插件：

**plugins.toml**：

```toml
shell = "zsh"

[templates]
source = """{% for file in files %}source "{{ file }}"\n{% endfor %}"""

# zsh-completions - 额外补全定义
[plugins.zsh-completions]
inline = '''fpath=(
  "/opt/homebrew/share/zsh-completions"
  "/opt/homebrew/Cellar/zsh/5.9/share/zsh/functions"
  $fpath
)'''

# zsh-syntax-highlighting - 语法高亮
[plugins.zsh-syntax-highlighting]
inline = 'source "/opt/homebrew/opt/zsh-syntax-highlighting/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh"'

# zsh-autosuggestions - 自动建议
[plugins.zsh-autosuggestions]
inline = 'source "/opt/homebrew/opt/zsh-autosuggestions/share/zsh-autosuggestions/zsh-autosuggestions.zsh"'

# Starship 提示符
[plugins.starship]
inline = 'eval "$(starship init zsh)"'

# zoxide 目录跳转
[plugins.zoxide]
inline = 'eval "$(zoxide init zsh)"'
```

**~/.zshrc** 只需一行：

```bash
eval "$(command sheldon source)"
```

### 常用别名

```bash
# 文件操作
alias ll="ls -lah"
alias la="ls -A"
alias ..="cd .."
alias ...="cd ../.."

# git 别名
alias g="git"
alias gs="git status"
alias ga="git add"
alias gc="git commit"
alias gco="git checkout"
alias gb="git branch"
alias gl="git log --oneline -20"
alias gp="git push"

# pnpm 别名
alias p="pnpm"
alias pi="pnpm install"
alias pd="pnpm dev"
alias pb="pnpm build"
```

## 三、编辑器与终端

### Cursor

- 基于 VS Code 的 AI 编辑器
- 内置 Claude、GPT-4 等模型
- 支持代码补全、对话、Diff 查看

### Ghostty

**config**：

```ini
# 窗口
window-width = 120
window-height = 40
background-opacity = 0.95

# 字体
font-family = "JetBrainsMono Nerd Font"
font-size = 14

# 链接打开（Cmd+Click）
open-url = true
open-url-modifier = cmd

# 行为
copy-on-select = true
confirm-close-surface = true
```

## 四、AI 工具

### Claude Code

```bash
# 安装
brew install claude-code

# 认证
claude auth

# 使用
claude "帮我修复这个 bug"
```

### Codex CLI

```bash
# 安装
brew install codex

# 认证
codex login

# 使用
codex "分析这个项目结构"
```

## 五、Dotfiles 管理

### Chezmoi

```bash
# 安装
brew install chezmoi

# 初始化
chezmoi init username

# 添加文件
chezmoi add ~/.zshrc

# 应用配置
chezmoi apply

# Git 操作
chezmoi git add .
chezmoi git commit -m "update config"
chezmoi git push
```

### 加密敏感文件

```bash
# GPG 加密
gpg --full-generate-key
chezmoi add --encrypt ~/.wakatime.cfg
```

## 六、开发环境

### Node.js

```bash
# 安装 fnm
brew install fnm

# 配置（~/.zshrc）
eval "$(fnm env --use-on-cd)"

# 使用
fnm use --lts
pnpm install
```

### Python

```bash
# 安装 uv
brew install uv

# 创建虚拟环境
uv venv
source .venv/bin/activate

# 安装依赖
uv pip install -r requirements.txt
```

### Java

```bash
# 安装 SDKMAN
brew install sdkman-cli
source "$SDKMAN_DIR/bin/sdkman-init.sh"

# 安装 JDK
sdk install java 17.0.9-tem

# 安装 Maven
sdk install maven
```

## 七、效率工具

### 数据库

- **TablePlus** - 多数据库客户端
- **OrbStack** - Docker 替代，轻量快速

### 网络

- **SwitchHosts** - hosts 文件管理
- **Insomnia** - API 调试工具

### 文件管理

- **Yazi** - 终端文件管理器
- **zoxide** - 智能目录跳转

## 八、快速恢复

新机器一键恢复：

```bash
# 1. 安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. 安装 chezmoi
brew install chezmoi

# 3. 初始化 dotfiles
chezmoi init --apply chensoul

# 4. 安装所有 brew 包
brew bundle install

# 5. 验证
chezmoi doctor
```

## 九、参考资料

- [我的 Dotfiles 仓库](https://github.com/chensoul/dotfiles)
- [Chezmoi 官方文档](https://www.chezmoi.io/)
- [Sheldon 文档](https://sheldon.cli.rs/)
- [Ghostty 文档](https://ghostty.org/docs/)
