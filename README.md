<p align="center">
  <img src="https://img.shields.io/github/stars/cn-ryw/git-setup-tutorial?style=social" alt="Stars">
  <img src="https://img.shields.io/github/license/cn-ryw/git-setup-tutorial" alt="License">
  <img src="https://img.shields.io/github/last-commit/cn-ryw/git-setup-tutorial" alt="Last Commit">
  <img src="https://img.shields.io/github/languages/top/cn-ryw/git-setup-tutorial" alt="Top Language">
</p>

<h1 align="center">Git 从零到精通：安装、配置与 GitHub 上传完全指南</h1>

<p align="center">
  <strong>面向零基础初学者 | 手把手教学 | 含常见问题排查</strong>
</p>

<p align="center">
  <a href="#快速开始">快速开始</a> •
  <a href="#目录">目录</a> •
  <a href="#常用命令速查表">命令速查</a> •
  <a href="#常见问题排查">问题排查</a>
</p>

---

## 为什么需要这份教程？

如果你刚接触编程，或者一直用"拖拽上传"的方式管理代码，这份教程将带你掌握**专业开发者的代码管理方式**。

学完后你将能够：

- 在 Windows / macOS / Linux 上正确安装和配置 Git
- 用 SSH Key 安全连接 GitHub，告别每次输入密码
- 掌握 `add → commit → push` 标准工作流
- 理解分支、合并、.gitignore 等核心概念
- 独立解决推送失败、权限错误等常见问题

---

## 快速开始

> 如果你已有一定基础，只想快速查阅关键步骤：

```bash
# 1. 安装 Git → https://git-scm.com/downloads
# 2. 配置身份
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 3. 生成 SSH Key 并添加到 GitHub
ssh-keygen -t ed25519 -C "你的邮箱"
cat ~/.ssh/id_ed25519.pub          # 复制输出内容 → GitHub Settings → SSH Keys

# 4. 创建仓库并推送
git init
git add .
git commit -m "feat: 首次提交"
git remote add origin git@github.com:用户名/仓库名.git
git push -u origin main
```

---

## 目录

| 章节 | 内容 |
|:----:|------|
| 1 | [Git 与 GitHub 是什么](#1-git-与-github-是什么) |
| 2 | [安装 Git](#2-安装-git) — Windows · macOS · Linux |
| 3 | [Git 全局配置](#3-git-全局配置) |
| 4 | [配置 SSH Key](#4-配置-ssh-key) — 安全免密连接 |
| 5 | [创建本地仓库](#5-创建本地仓库) |
| 6 | [基本工作流](#6-基本工作流) — add → commit → push |
| 7 | [推送到 GitHub](#7-推送到-github) — 远程仓库操作 |
| 8 | [分支管理基础](#8-分支管理基础) |
| 9 | [.gitignore 文件](#9-gitignore-文件) |
| 10 | [常用命令速查表](#10-常用命令速查表) |
| 11 | [常见问题排查](#11-常见问题排查) |
| 附录 | [GitHub CLI 快速上手](#附录github-cli-gh-快速上手) |

---

## 1. Git 与 GitHub 是什么

| 概念 | 说明 |
|------|------|
| **Git** | 分布式版本控制系统。跟踪文件的每一次修改，可以随时回退到任意历史版本。运行在本地电脑上。 |
| **GitHub** | 基于 Git 的云端代码托管平台。提供远程仓库、Pull Request 代码审查、Issue 任务跟踪、Actions 自动化等协作功能。 |

> **一句话理解：** Git = 本地版本管理器；GitHub = 云端代码协作平台。

许多新手误以为是同一个东西——实际上你可以只用 Git 而不碰 GitHub，但**两者结合才是现代软件开发的标配**。

---

## 2. 安装 Git

### Windows

1. 访问 [git-scm.com/downloads/win](https://git-scm.com/downloads/win) 下载 `.exe` 安装包
2. 运行安装程序，**以下选项建议照此设置**（其余默认即可）：

| 步骤 | 建议选项 | 原因 |
|------|----------|------|
| 编辑器 | **VS Code** | 提交信息编辑体验好 |
| 默认分支名 | **Override → main** | 行业新标准 |
| PATH 环境 | **Git from the command line...** | 终端中随处可用 |
| HTTPS 传输 | **Use the OpenSSL library** | 兼容性最好 |
| 换行符 | **Checkout Windows-style, commit Unix-style** | 跨平台协作不乱码 |
| 终端模拟器 | **Use MinTTY** | 比 Windows 自带终端好用 |

3. 验证安装：
   ```bash
   git --version
   # 输出示例：git version 2.54.0.windows.1
   ```

### macOS

```bash
# 推荐：Homebrew 安装
brew install git

# 备选：Xcode 命令行工具
xcode-select --install
```

### Linux

```bash
# Debian / Ubuntu
sudo apt update && sudo apt install git

# Fedora
sudo dnf install git

# Arch
sudo pacman -S git
```

---

## 3. Git 全局配置

```bash
# 设置身份（会写入每次提交记录）
git config --global user.name "你的名字"
git config --global user.email "your-email@example.com"

# 查看全部配置
git config --global --list
```

> **务必让邮箱与 GitHub 注册邮箱一致**，否则 GitHub 无法将提交关联到你的账号，贡献面板上不会有绿点。

---

## 4. 配置 SSH Key

SSH Key 是**一劳永逸**的 GitHub 连接方式：配置一次，以后推送不再需要输入密码。

### 4.1 生成密钥

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

交互提示均按回车即可（使用默认路径和空密码）。  
旧系统若报错，改用：`ssh-keygen -t rsa -b 4096 -C "your-email@example.com"`

### 4.2 启动 ssh-agent 并添加密钥

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 4.3 将公钥添加到 GitHub

```bash
# 复制公钥
cat ~/.ssh/id_ed25519.pub
```

然后：GitHub → Settings → SSH and GPG keys → New SSH key → 粘贴 → 保存。

### 4.4 验证

```bash
ssh -T git@github.com
# 成功：Hi <用户名>! You've successfully authenticated.
```

---

## 5. 创建本地仓库

```bash
mkdir my-project && cd my-project
git init                        # 初始化，生成 .git 目录
echo "# My Project" > README.md
git status                      # 随时查看仓库状态
```

---

## 6. 基本工作流

Git 的核心流转模型——理解这张图就理解了 Git：

```
┌──────────────┐     git add     ┌──────────┐    git commit    ┌────────────┐
│   工作区      │ ──────────────→ │  暂存区   │ ──────────────→ │  本地仓库   │
│ (Working Dir) │                │ (Stage)   │                 │  (Repo)     │
└──────────────┘                └──────────┘                 └──────┬─────┘
                                                                    │
                                                            git push │  git pull
                                                                    │
                                                               ┌────▼──────┐
                                                               │  远程仓库  │
                                                               │ (GitHub)   │
                                                               └───────────┘
```

### 6.1 添加 → 暂存区

```bash
git add README.md    # 指定文件
git add .            # 全部改动
```

### 6.2 提交 → 本地仓库

```bash
git commit -m "feat: 完成登录功能"

# 多行提交信息
git commit -m "feat: 添加用户登录

实现用户名密码登录，含表单验证和错误提示。"
```

**约定式提交前缀：**

| 前缀 | 含义 | 示例 |
|------|------|------|
| `feat:` | 新功能 | `feat: 添加搜索` |
| `fix:` | Bug 修复 | `fix: 修复登录超时` |
| `docs:` | 文档 | `docs: 更新安装说明` |
| `refactor:` | 重构 | `refactor: 简化认证逻辑` |
| `test:` | 测试 | `test: 补充登录用例` |
| `chore:` | 杂项 | `chore: 升级依赖` |

### 6.3 查看历史

```bash
git log --oneline                     # 一行一提交
git log --oneline --graph --all       # 图形 + 所有分支
```

---

## 7. 推送到 GitHub

### 7.1 创建远程仓库

**网页创建：** [github.com/new](https://github.com/new) → 填入名称 → **不要勾选** "Add README" → Create。

**命令行创建（推荐）：**
```bash
gh repo create my-project --public --source=. --remote=origin --push
```

### 7.2 关联并推送

```bash
git remote add origin git@github.com:你的用户名/仓库名.git
git push -u origin main    # 首次推送（-u 绑定了上游，后续只需 git push）
```

### 7.3 克隆已有仓库

```bash
git clone git@github.com:用户名/仓库名.git    # SSH（推荐）
git clone https://github.com/用户名/仓库名.git # HTTPS
```

---

## 8. 分支管理基础

```bash
git branch feature-payment       # 创建分支
git checkout feature-payment     # 切换过去
git checkout -b feature-payment  # 创建 + 切换（一步到位）

# 合并回主线
git checkout main
git merge feature-payment

git branch -d feature-payment    # 删除已合并的分支
```

**分支示意图：**
```
main    ──●──────●──────●────────  （稳定版本）
            \         /
feature     ●──●──●──              （独立开发，互不干扰）
```

---

## 9. .gitignore 文件

以下类型的文件**不应提交**到仓库——在项目根目录创建 `.gitignore`：

```gitignore
# 依赖
node_modules/   vendor/

# 构建产物
dist/   build/   *.exe   *.dll

# 密钥与环境变量
.env   .env.local   *.pem

# IDE 配置
.vscode/   .idea/   *.swp

# 系统文件
.DS_Store   Thumbs.db
```

> 更多模板：[github/gitignore](https://github.com/github/gitignore)

---

## 10. 常用命令速查表

| 命令 | 作用 |
|------|------|
| `git init` | 初始化本地仓库 |
| `git clone <url>` | 克隆远程仓库 |
| `git status` | 查看工作区与暂存区状态 |
| `git add <file>` / `git add .` | 添加文件到暂存区 / 全部添加 |
| `git commit -m "msg"` | 提交到本地仓库 |
| `git push` / `git push -u origin main` | 推送 / 首次推送并绑定上游 |
| `git pull` / `git pull --rebase` | 拉取合并 / 变基方式拉取 |
| `git fetch` | 下载远程更新但不合并 |
| `git branch` / `git branch -a` | 本地分支 / 含远程分支 |
| `git checkout <branch>` | 切换分支 |
| `git checkout -b <name>` | 创建并切换到新分支 |
| `git merge <branch>` | 合并分支 |
| `git log --oneline --graph --all` | 图形化提交历史 |
| `git diff` / `git diff --staged` | 工作区差异 / 暂存区差异 |
| `git reset --soft HEAD~1` | 撤销最近 commit（保留文件） |
| `git stash` / `git stash pop` | 暂存修改 / 恢复 |
| `git remote -v` | 查看远程仓库地址 |
| `git rm --cached <file>` | 停止跟踪（保留本地文件） |

---

## 11. 常见问题排查

<details open>
<summary><strong>Q1：推送报 "Permission denied (publickey)"</strong></summary>

```bash
ssh-add -l                      # 检查已加载的密钥
ssh-add ~/.ssh/id_ed25519       # 重新加载
ssh -T git@github.com           # 测试连接
```
</details>

<details>
<summary><strong>Q2：GitHub 贡献面板不显示我的提交</strong></summary>

本地邮箱与 GitHub 注册邮箱不一致导致。检查并修正：
```bash
git config user.email                           # 查看
git config --global user.email "正确邮箱"         # 修正
```
> 只会影响以后的提交，历史提交无法追溯修改。
</details>

<details>
<summary><strong>Q3：推送被拒 "Updates were rejected"</strong></summary>

远程有新提交而本地没有，先拉取再推送：
```bash
git pull --rebase origin main
git push
```
</details>

<details>
<summary><strong>Q4：撤销最近一次 commit</strong></summary>

```bash
git reset --soft HEAD~1    # 撤销 commit，文件改动保留在暂存区
git reset --hard HEAD~1    # 撤销 commit，文件改动也丢弃 ⚠️ 不可恢复
```
</details>

<details>
<summary><strong>Q5：误提交了不该提交的文件</strong></summary>

```bash
git rm --cached 文件名
echo "文件名" >> .gitignore
git commit -m "chore: 移除误提交的文件"
```
</details>

<details>
<summary><strong>Q6：修改远程仓库地址</strong></summary>

```bash
git remote set-url origin git@github.com:新用户名/新仓库.git
```
</details>

---

## 附录：GitHub CLI (`gh`) 快速上手

```bash
# 安装
winget install --id GitHub.cli     # Windows
brew install gh                    # macOS

# 认证
gh auth login

# 日常操作
gh repo create my-project --public --clone    # 创建仓库
gh repo view 用户名/仓库名                     # 查看仓库信息
gh issue create --title "标题" --body "内容"   # 创建 Issue
gh pr create --title "标题" --body "内容"      # 创建 Pull Request
```

---

<br>

<p align="center">
  <sub>如果这份教程对你有帮助，请给一个 ⭐ Star 支持一下！</sub>
</p>

<p align="center">
  <sub>Made with ❤️ by <a href="https://github.com/cn-ryw">cn-ryw</a></sub>
</p>
