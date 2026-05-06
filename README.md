# Git 安装、配置与上传 GitHub 完整教程

> 面向零基础用户，手把手教你从安装 Git 到将代码推送到 GitHub。

---

## 目录

1. [Git 与 GitHub 是什么](#1-git-与-github-是什么)
2. [安装 Git](#2-安装-git)
3. [Git 全局配置](#3-git-全局配置)
4. [配置 SSH Key](#4-配置-ssh-key)
5. [创建本地仓库](#5-创建本地仓库)
6. [基本工作流](#6-基本工作流)
7. [推送到 GitHub](#7-推送到-github)
8. [分支管理基础](#8-分支管理基础)
9. [.gitignore 文件](#9-gitignore-文件)
10. [常用命令速查表](#10-常用命令速查表)
11. [常见问题排查](#11-常见问题排查)

---

## 1. Git 与 GitHub 是什么

| 概念 | 说明 |
|------|------|
| **Git** | 分布式版本控制系统，用于跟踪文件变化、协作开发。运行在本地。 |
| **GitHub** | 基于 Git 的代码托管平台（云端），提供远程仓库、PR 审查、Issue 跟踪等功能。 |

**简单类比：** Git 是你电脑上的"存档管理器"；GitHub 是云端"存档共享平台"。

---

## 2. 安装 Git

### Windows

1. 访问 [git-scm.com](https://git-scm.com/downloads/win) 下载安装包
2. 运行安装程序，推荐选项：
   - 编辑器选择：**VS Code**（或其他你喜欢的编辑器）
   - 默认分支名：选择 **main**（或保持 `master`，建议用 `main`）
   - PATH 环境：选择 **"Git from the command line and also from 3rd-party software"**
   - HTTPS 传输：选择 **"Use the OpenSSL library"**
   - 换行符处理：选择 **"Checkout Windows-style, commit Unix-style line endings"**
   - 终端模拟器：选择 **"Use MinTTY"**
   - 其余选项保持默认，一路 Next
3. 安装完成后，打开终端验证：
   ```bash
   git --version
   # 输出示例：git version 2.54.0.windows.1
   ```

### macOS

```bash
# 方式一：通过 Homebrew 安装（推荐）
brew install git

# 方式二：通过 Xcode Command Line Tools
xcode-select --install
```

### Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install git
```

---

## 3. Git 全局配置

安装完成后，首先配置你的身份信息。这些信息会出现在每次提交记录中。

```bash
# 设置用户名
git config --global user.name "你的名字"

# 设置邮箱（务必与 GitHub 注册邮箱一致）
git config --global user.email "your-email@example.com"

# 查看配置
git config --global --list
```

**为什么要与 GitHub 邮箱一致？** GitHub 通过邮箱将提交记录关联到你的账号。邮箱不一致会导致提交记录不被识别为你的贡献。

---

## 4. 配置 SSH Key

SSH Key 让你无需每次输入密码即可安全连接 GitHub。

### 4.1 生成 SSH Key

```bash
# 替换为你的 GitHub 邮箱
ssh-keygen -t ed25519 -C "your-email@example.com"
```

交互提示：
- `Enter file in which to save the key` → 直接回车（使用默认路径 `~/.ssh/id_ed25519`）
- `Enter passphrase` → 可直接回车（不设密码）或输入密码

> 如果系统较旧不支持 Ed25519，可用 `ssh-keygen -t rsa -b 4096 -C "your-email@example.com"`

### 4.2 添加 SSH Key 到 ssh-agent

```bash
# 启动 ssh-agent
eval "$(ssh-agent -s)"

# 添加私钥
ssh-add ~/.ssh/id_ed25519
```

### 4.3 将公钥添加到 GitHub

```bash
# 复制公钥内容
cat ~/.ssh/id_ed25519.pub
```

然后：
1. 打开 GitHub → 右上角头像 → **Settings**
2. 左侧菜单 → **SSH and GPG keys**
3. 点击 **New SSH key**
4. Title 填写一个便于识别的名称（如"我的 Windows 电脑"）
5. Key 粘贴刚才复制的公钥内容
6. 点击 **Add SSH key**

### 4.4 测试连接

```bash
ssh -T git@github.com
# 成功输出：Hi 用户名! You've successfully authenticated.
```

---

## 5. 创建本地仓库

```bash
# 创建项目文件夹
mkdir my-project
cd my-project

# 初始化 Git 仓库
git init

# 创建第一个文件
echo "# My Project" > README.md

# 查看仓库状态
git status
```

---

## 6. 基本工作流

Git 的日常操作遵循 **三步走** 流程：

```
工作区 (Working Directory)
    │  git add
    ▼
暂存区 (Staging Area)
    │  git commit
    ▼
本地仓库 (Local Repository)
    │  git push
    ▼
远程仓库 (Remote Repository)
```

### 6.1 添加到暂存区

```bash
# 添加指定文件
git add README.md

# 添加所有改动
git add .

# 查看暂存状态
git status
```

### 6.2 提交到本地仓库

```bash
# 简单提交
git commit -m "feat: 初始化项目"

# 详细提交
git commit -m "feat: 添加用户登录功能

实现了用户名密码登录，包含表单验证和错误提示。"
```

**提交信息规范（约定式提交）：**

| 类型 | 用途 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 bug |
| `docs` | 文档变更 |
| `refactor` | 代码重构 |
| `test` | 测试相关 |
| `chore` | 杂项（依赖更新等） |

### 6.3 查看历史

```bash
# 提交历史
git log --oneline

# 图形化历史（含分支）
git log --oneline --graph --all
```

---

## 7. 推送到 GitHub

### 7.1 在 GitHub 上创建仓库

**方式一：网页创建**
1. 打开 [github.com/new](https://github.com/new)
2. 填写 Repository name
3. 选择 Public 或 Private
4. **不要勾选** "Add a README file"（因为本地已有）
5. 点击 Create repository

**方式二：命令行创建（需安装 GitHub CLI）**
```bash
gh repo create my-project --public --source=. --remote=origin --push
```

### 7.2 关联远程仓库并推送

```bash
# 关联远程仓库（推荐 SSH 方式）
git remote add origin git@github.com:你的用户名/仓库名.git

# 首次推送（-u 建立上游关联，后续只需 git push）
git push -u origin main

# 后续推送
git push
```

### 7.3 克隆已有仓库到本地

```bash
# SSH 方式（推荐）
git clone git@github.com:用户名/仓库名.git

# HTTPS 方式
git clone https://github.com/用户名/仓库名.git
```

---

## 8. 分支管理基础

```bash
# 查看所有分支
git branch -a

# 创建新分支
git branch feature-login

# 切换分支
git checkout feature-login

# 创建并切换（一步到位）
git checkout -b feature-login

# 合并分支到 main
git checkout main
git merge feature-login

# 删除已合并的分支
git branch -d feature-login
```

**典型分支工作流：**
```
main ──────●──────●──────●──── (稳定主线)
            \         /
feature     ●──●──●──  (功能开发)
```

---

## 9. .gitignore 文件

有些文件不应纳入版本控制（编译产物、依赖包、密钥等）。

在项目根目录创建 `.gitignore`：

```gitignore
# 依赖目录
node_modules/
vendor/

# 编译产物
dist/
build/
*.exe
*.dll

# 环境变量（含密钥）
.env
.env.local

# IDE 配置
.vscode/
.idea/

# 操作系统文件
.DS_Store
Thumbs.db
```

> GitHub 提供各语言模板：[github/gitignore](https://github.com/github/gitignore)

---

## 10. 常用命令速查表

| 命令 | 作用 |
|------|------|
| `git init` | 初始化仓库 |
| `git clone <url>` | 克隆远程仓库 |
| `git status` | 查看工作区状态 |
| `git add <file>` | 添加文件到暂存区 |
| `git commit -m "msg"` | 提交到本地仓库 |
| `git push` | 推送到远程 |
| `git pull` | 拉取远程更新并合并 |
| `git fetch` | 获取远程信息（不合并） |
| `git branch` | 查看分支列表 |
| `git checkout <branch>` | 切换分支 |
| `git checkout -b <name>` | 创建并切换分支 |
| `git merge <branch>` | 合并指定分支到当前分支 |
| `git log --oneline` | 查看简洁提交历史 |
| `git diff` | 查看未暂存的改动 |
| `git reset --soft HEAD~1` | 撤销最近一次 commit（保留修改） |
| `git stash` / `git stash pop` | 暂存当前工作 / 恢复 |
| `git remote -v` | 查看远程仓库地址 |

---

## 11. 常见问题排查

### Q1：推送时报 "Permission denied (publickey)"

```bash
# 检查 ssh-agent 中是否有密钥
ssh-add -l

# 重新添加
ssh-add ~/.ssh/id_ed25519

# 测试 GitHub 连接
ssh -T git@github.com
```

### Q2：提交记录不显示在 GitHub 贡献面板上

原因：本地配置的邮箱与 GitHub 注册邮箱不一致。

```bash
# 查看当前邮箱
git config user.email

# 修正为 GitHub 注册邮箱
git config --global user.email "正确的邮箱@example.com"
# 注意：只影响以后的提交，历史提交不会改变
```

### Q3：推送被拒绝 "Updates were rejected"

```bash
# 远程有本地没有的更新，先拉取再推送
git pull --rebase origin main
git push
```

### Q4：想撤销最近一次 commit（保留文件改动）

```bash
git reset --soft HEAD~1
```

### Q5：不小心提交了不该提交的文件

```bash
# 从 Git 跟踪中移除（保留本地文件）
git rm --cached 文件名
# 添加到 .gitignore 防止再次提交
echo "文件名" >> .gitignore
git commit -m "chore: 移除误提交的文件"
```

### Q6：修改远程仓库地址

```bash
git remote set-url origin git@github.com:新用户名/新仓库.git
```

---

## 附录：GitHub CLI (`gh`) 快速上手

```bash
# Windows 安装
winget install --id GitHub.cli

# 认证登录
gh auth login

# 查看登录状态
gh auth status

# 命令行创建仓库
gh repo create my-repo --public --clone
```

---

> **学完本教程，你已掌握 Git 的核心操作。** 日常项目中坚持使用 Git 进行版本管理，熟练后可以进一步学习 rebase、cherry-pick、子模块等进阶主题。
