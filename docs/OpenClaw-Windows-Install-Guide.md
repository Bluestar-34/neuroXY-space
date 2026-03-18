# OpenClaw Windows 安装完整指南

> 整理日期：2026-03-18
> 适用版本：2026.3+
> 作者：neuroXY

---

## 目录

1. [环境准备](#1-环境准备)
2. [安装 OpenClaw](#2-安装-openclaw)
3. [首次配置](#3-首次配置)
4. [配置模型](#4-配置模型)
5. [配置通道（可选）](#5-配置通道可选)
6. [启动与验证](#6-启动与验证)
7. [常用操作](#7-常用操作)
8. [常见问题](#8-常见问题)

---

## 1. 环境准备

### 1.1 安装 Node.js

OpenClaw 需要 **Node.js 22+**（推荐 Node 24）

```powershell
# 检查是否已安装
node --version

# 如果没有，安装 Node.js
# 方法1：使用 winget（推荐）
winget install OpenJS.NodeJS.LTS

# 方法2：手动下载安装
# https://nodejs.org/
```

### 1.2 安装 Git（可选但推荐）

用于版本管理和克隆仓库

```powershell
# 检查是否已安装
git --version

# 如果没有，安装 Git
winget install Git.Git
```

### 1.3 安装 Python（部分技能需要）

```powershell
# 检查是否已安装
python --version

# 如果没有，安装 Python 3.11+
winget install Python.Python.3.11
```

---

## 2. 安装 OpenClaw

### 2.1 一键安装（推荐）

打开 **PowerShell**（以管理员身份运行），执行：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 2.2 验证安装

```powershell
# 查看版本
openclaw --version

# 查看帮助
openclaw --help
```

### 2.3 手动安装（备选）

如果一键安装失败：

```powershell
# 创建安装目录
mkdir ~/.openclaw; cd ~/.openclaw

# 克隆仓库
git clone https://github.com/openclaw/openclaw.git

# 安装依赖
cd openclaw
npm install

# 链接命令
npm link
```

---

## 3. 首次配置

### 3.1 运行引导程序

```powershell
openclaw setup
```

或

```powershell
openclaw onboard --install-daemon
```

### 3.2 交互式配置

根据提示完成以下配置：

1. **工作区路径** - 选择你的工作目录
2. **默认模型** - 选择使用的 AI 模型
3. **网关设置** - 端口、绑定模式
4. **通道选择** - 可选配置

### 3.3 配置文件位置

- 主配置：`~/.openclaw/openclaw.json`
- 工作区：`~/.openclaw/workspace/`
- 密钥：`~/.openclaw/secrets.json`

---

## 4. 配置模型

### 4.1 查看可用模型

```powershell
openclaw models list
```

### 4.2 配置 MiniMax（推荐国内使用）

```powershell
# 方式1：交互式配置
openclaw configure --section model

# 方式2：手动配置
# 获取 API Key: https://platform.minimaxi.com/user-center/interface-key

# 设置 API Key
openclaw config set models.providers.minimax-cn.apiKey "你的API密钥"

# 设置默认模型
openclaw config set agents.defaults.model.primary "minimax-cn/MiniMax-M2.5"
```

### 4.3 配置 OpenAI（备选）

```powershell
# 设置 API Key
openclaw config set models.providers.openai.apiKey "sk-..."

# 设置默认模型
openclaw config set agents.defaults.model.primary "openai/gpt-4o"
```

### 4.4 模型认证

```powershell
# 添加认证
openclaw models auth add minimax-cn

# 查看认证状态
openclaw models status
```

---

## 5. 配置通道（可选）

### 5.1 支持的通道

| 通道 | 命令 | 说明 |
|------|------|------|
| Discord | `discord` | Discord 机器人 |
| Telegram | `telegram` | Telegram 机器人 |
| 飞书 | `feishu` | 飞书应用 |
| QQ | `qqbot` | QQ 机器人 |
| WhatsApp | `whatsapp` | WhatsApp |
| Slack | `slack` | Slack |

### 5.2 配置示例：飞书

```powershell
# 交互式配置
openclaw configure --section channels
# 选择 feishu

# 或手动配置
# 1. 创建飞书应用：https://open.feishu.cn/
# 2. 获取 App ID 和 App Secret
# 3. 配置
openclaw config set channels.feishu.appId "你的AppID"
openclaw config set channels.feishu.appSecret "你的AppSecret"
```

### 5.3 配置示例：QQ

```powershell
# 交互式配置
openclaw configure --section channels
# 选择 qqbot

# 或手动配置
# 1. 创建 QQ 机器人：https://q.qq.com/
# 2. 获取 App ID 和 Client Secret
# 3. 配置
openclaw config set channels.qqbot.appId "你的AppID"
openclaw config set channels.qqbot.clientSecret "你的ClientSecret"
```

### 5.4 启用通道

```powershell
# 查看通道状态
openclaw channels status

# 启用通道
openclaw config set channels.feishu.enabled true
```

---

## 6. 启动与验证

### 6.1 启动网关

```powershell
# 启动网关（前台，用于调试）
openclaw gateway run

# 启动网关（后台服务）
openclaw gateway start

# 或安装为系统服务
openclaw gateway install
```

### 6.2 验证运行状态

```powershell
# 查看网关状态
openclaw gateway status

# 健康检查
openclaw health

# 完整状态
openclaw status
```

### 6.3 访问控制面板

```powershell
# 自动打开浏览器
openclaw dashboard

# 或手动访问
# 本地：http://127.0.0.1:18789/
```

### 6.4 测试发送消息

```powershell
# 发送到配置好的通道
openclaw message send --target <目标> --message "测试消息"
```

---

## 7. 常用操作

### 7.1 网关管理

```powershell
# 启动/停止/重启
openclaw gateway start
openclaw gateway stop
openclaw gateway restart

# 查看日志
openclaw logs
openclaw logs --follow
```

### 7.2 配置管理

```powershell
# 查看配置
openclaw config file
openclaw config get agents.defaults.workspace

# 设置配置
openclaw config set gateway.port 18789

# 验证配置
openclaw config validate
```

### 7.3 技能管理

```powershell
# 安装 ClawHub
npm i -g clawhub

# 搜索技能
clawhub search "github"

# 安装技能
clawhub install github-cli

# 列出已安装
openclaw skills list
```

### 7.4 插件管理

```bash
# 列出插件
openclaw plugins list

# 安装插件
openclaw plugins install @openclaw/feishu

# 启用/禁用
openclaw plugins enable feishu
openclaw plugins disable feishu
```

### 7.5 备份与恢复

```powershell
# 创建备份
openclaw backup create

# 验证备份
openclaw backup verify
```

---

## 8. 常见问题

### Q1: 安装后命令找不到

```powershell
# 刷新环境变量
refreshenv

# 或重新打开 PowerShell

# 检查安装
openclaw --version
```

### Q2: 网关启动失败

```powershell
# 查看详细错误
openclaw logs

# 检查端口占用
netstat -ano | findstr 18789

# 更换端口
openclaw config set gateway.port 18790
```

### Q3: 模型连接失败

```powershell
# 检查 API Key
openclaw config get models.providers.minimax-cn.apiKey

# 测试连接
openclaw models status

# 查看日志
openclaw logs
```

### Q4: 通道无法登录

```powershell
# 检查通道配置
openclaw channels status

# 查看通道日志
openclaw channels logs <channel-name>

# 重新配置
openclaw configure --section channels
```

### Q5: 需要代理才能访问外网

```powershell
# 设置代理
$env:HTTP_PROXY = "http://127.0.0.1:7890"
$env:HTTPS_PROXY = "http://127.0.0.1:7890"

# 或在配置中设置
openclaw config set http.proxy "http://127.0.0.1:7890"
```

### Q6: 完全重置

```powershell
# 停止网关
openclaw gateway stop

# 重置配置
openclaw reset

# 重新配置
openclaw onboard
```

---

## 附录

### A. 目录结构

```
~/.openclaw/
├── openclaw.json       # 主配置文件
├── secrets.json       # 密钥文件
├── workspace/         # 工作区
├── extensions/       # 扩展插件
└── logs/            # 日志文件
```

### B. 端口说明

| 端口 | 默认用途 |
|------|----------|
| 18789 | Gateway WebSocket |
| 18790 | 备用端口 |

### C. 常用命令速查

```powershell
# 快速开始
openclaw setup                    # 首次配置
openclaw dashboard                # 打开控制面板
openclaw status                   # 查看状态
openclaw health                   # 健康检查
openclaw gateway restart          # 重启网关

# 配置
openclaw configure                # 交互式配置
openclaw config get <path>        # 查看配置
openclaw config set <path> <val>  # 设置配置
```

---

## 更新日志

- **2026-03-18**: 初始版本

---

*由 neuroXY 整理 🦐*
