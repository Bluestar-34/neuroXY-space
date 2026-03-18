# OpenClaw 核心命令汇总

> 整理日期：2026-03-18
> 版本：2026.3.11+
> 作者：neuroXY

---

## 目录

1. [基础命令](#1-基础命令)
2. [配置管理](#2-配置管理)
3. [网关管理](#3-网关管理)
4. [插件管理](#4-插件管理)
5. [技能管理](#5-技能管理)
6. [会话管理](#6-会话管理)
7. [模型管理](#7-模型管理)
8. [通道管理](#8-通道管理)
9. [定时任务](#9-定时任务)
10. [节点管理](#10-节点管理)
11. [安全与密钥](#11-安全与密钥)
12. [日志与监控](#12-日志与监控)
13. [快捷命令](#13-快捷命令)

---

## 1. 基础命令

### 1.1 安装与初始化

| 命令 | 说明 | 示例 |
|------|------|------|
| `openclaw setup` | 首次安装引导 | `openclaw setup` |
| `openclaw onboard` | 重新配置引导 | `openclaw onboard` |
| `openclaw configure` | 交互式配置 | `openclaw configure` |

#### configure 子选项

```bash
# 配置特定模块
openclaw configure --section workspace     # 工作区
openclaw configure --section model          # 模型
openclaw configure --section web            # 网页
openclaw configure --section gateway        # 网关
openclaw configure --section daemon         # 守护进程
openclaw configure --section channels       # 通道
openclaw configure --section skills         # 技能
openclaw configure --section health         # 健康检查
```

### 1.2 版本与帮助

```bash
openclaw --version          # 查看版本
openclaw --help             # 查看帮助
openclaw <command> --help   # 查看子命令帮助
```

### 1.3 全局选项

| 选项 | 说明 |
|------|------|
| `--dev` | 开发模式，使用 `~/.openclaw-dev` 目录 |
| `--profile <name>` | 使用指定配置文件 `~/.openclaw-<name>` |
| `--no-color` | 禁用颜色输出 |
| `--json` | JSON 格式输出 |
| `-V, --version` | 打印版本 |

---

## 2. 配置管理

### 2.1 核心配置命令

```bash
# 查看当前配置文件路径
openclaw config file

# 获取配置项
openclaw config get <path>
openclaw config get agents.defaults.workspace
openclaw config get agents.list[0].id

# 设置配置项
openclaw config set <path> <value>
openclaw config set gateway.port 19001
openclaw config set agents.defaults.heartbeat.every "2h"

# 删除配置项
openclaw config unset <path>

# 验证配置文件
openclaw config validate
openclaw config validate --json
```

### 2.2 路径语法

- **点号表示法**：`agents.defaults.workspace`
- **括号表示法**：`agents.list[0].id`
- **索引访问**：`agents.list[1].tools.exec.node`

### 2.3 配置模式

```bash
# 1. 直接赋值
openclaw config set gateway.port 19001

# 2. 密钥引用模式
openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN

# 3. 批量模式
openclaw config set --batch-json '[{"path": "...", "value": "..."}]'
```

### 2.4 配置重置与备份

```bash
# 备份
openclaw backup create

# 验证备份
openclaw backup verify

# 重置
openclaw reset
openclaw reset --all

# 卸载
openclaw uninstall
```

---

## 3. 网关管理

### 3.1 启动与停止

```bash
# 启动网关（前台）
openclaw gateway run

# 启动网关（后台）
openclaw gateway start

# 停止网关
openclaw gateway stop

# 重启网关
openclaw gateway restart

# 查看网关状态
openclaw gateway status
```

### 3.2 网关选项

```bash
# 指定端口
openclaw gateway run --port 18789

# 指定绑定模式
openclaw gateway run --bind loopback    # 本地
openclaw gateway run --bind lan         # 局域网
openclaw gateway run --bind tailnet     # Tailscale

# 认证模式
openclaw gateway run --auth token
openclaw gateway run --auth password
openclaw gateway run --token <token>

# 开发模式
openclaw gateway run --dev
openclaw gateway run --dev --reset
```

### 3.3 网关查询

```bash
# 健康检查
openclaw gateway health

# 探测网关
openclaw gateway probe

# 发现网关
openclaw gateway discover

# WebSocket 调用
openclaw gateway call <method> [params]
```

---

## 4. 插件管理

### 4.1 插件列表与信息

```bash
# 列出已安装插件
openclaw plugins list

# 查看插件详情
openclaw plugins info <plugin-name>

# 查看插件配置
openclaw plugins doctor
```

### 4.2 插件安装与卸载

```bash
# 安装插件
openclaw plugins install <package>
openclaw plugins install @openclaw/feishu
openclaw plugins install ./local-plugin

# 卸载插件
openclaw plugins uninstall <plugin-name>

# 更新插件
openclaw plugins update
openclaw plugins update <plugin-name>
```

### 4.3 插件启用/禁用

```bash
# 启用插件
openclaw plugins enable <plugin-name>

# 禁用插件
openclaw plugins disable <plugin-name>
```

### 4.4 插件市场

```bash
# 列出可用插件
openclaw plugins marketplace list

# 从市场安装
openclaw plugins install <plugin-name>@<marketplace>
```

---

## 5. 技能管理

### 5.1 技能列表与信息

```bash
# 列出所有技能
openclaw skills list

# 查看技能详情
openclaw skills info <skill-name>

# 检查技能状态
openclaw skills check
```

### 5.2 ClawHub 技能市场

```bash
# 搜索技能
clawhub search <keyword>

# 安装技能
clawhub install <skill-name>

# 更新技能
clawhub update <skill-name>
clawhub update --all

# 列出已安装
clawhub list

# 发布技能
clawhub publish ./my-skill --slug <slug> --name "My Skill" --version 1.0.0
```

---

## 6. 会话管理

### 6.1 会话查看

```bash
# 列出所有会话
openclaw sessions

# 查看会话历史
openclaw sessions history

# 查看特定会话
openclaw sessions <session-key>
```

### 6.2 代理/Agent 管理

```bash
# 列出 Agents
openclaw agents list

# 添加 Agent
openclaw agents add <agent-id>

# 删除 Agent
openclaw agents delete <agent-id>
```

### 6.3 ACP 协议

```bash
# ACP 相关命令
openclaw acp <subcommand>
```

---

## 7. 模型管理

### 7.1 模型列表与状态

```bash
# 列出可用模型
openclaw models list

# 查看模型状态
openclaw models status

# 扫描新模型
openclaw models scan
```

### 7.2 模型配置

```bash
# 设置默认模型
openclaw models set <model-name>

# 设置默认图像模型
openclaw models set-image <model-name>

# 模型别名
openclaw models aliases list
openclaw models aliases add <alias> <model>
openclaw models aliases remove <alias>

# 备用模型
openclaw models fallbacks list
openclaw models fallbacks add <model>
openclaw models fallbacks clear
```

### 7.3 模型认证

```bash
# 添加认证
openclaw models auth add <provider>

# 设置 Token
openclaw models auth setup-token <provider>

# 粘贴 Token
openclaw models auth paste-token <provider>

# 认证顺序
openclaw models auth order get
openclaw models auth order set <provider1> <provider2>
openclaw models auth order clear
```

---

## 8. 通道管理

### 8.1 通道列表与状态

```bash
# 列出所有通道
openclaw channels

# 查看通道状态
openclaw channels status

# 查看通道日志
openclaw channels logs
```

### 8.2 通道操作

```bash
# 添加通道
openclaw channels add <channel-type>

# 移除通道
openclaw channels remove <channel-name>

# 登录通道
openclaw channels login <channel-type>

# 登出通道
openclaw channels logout <channel-name>
```

### 8.3 支持的通道

- `discord` - Discord
- `telegram` - Telegram
- `whatsapp` - WhatsApp
- `slack` - Slack
- `feishu` - 飞书
- `qqbot` - QQ
- `signal` - Signal
- `sms` - SMS

---

## 9. 定时任务

### 9.1 Cron 任务管理

```bash
# 查看任务状态
openclaw cron status

# 列出所有任务
openclaw cron list

# 添加任务
openclaw cron add <job-config>

# 编辑任务
openclaw cron edit <job-id>

# 删除任务
openclaw cron rm <job-id>

# 启用/禁用任务
openclaw cron enable <job-id>
openclaw cron disable <job-id>

# 立即执行任务
openclaw cron run <job-id>
```

### 9.2 Cron 表达式示例

```bash
# 每小时
openclaw cron add --schedule "0 * * * *" --command "..."

# 每天早上9点
openclaw cron add --schedule "0 9 * * *" --command "..."

# 每30分钟
openclaw cron add --schedule "*/30 * * * *" --command "..."
```

---

## 10. 节点管理

### 10.1 节点查看

```bash
# 列出所有节点
openclaw nodes

# 查看节点状态
openclaw nodes status

# 查看节点详情
openclaw node <node-id>
```

### 10.2 节点配对

```bash
# 配对新节点
openclaw pairing start

# 完成配对
openclaw pairing complete <code>
```

### 10.3 设备管理

```bash
# 设备列表
openclaw devices

# 设备状态
openclaw devices status
```

---

## 11. 安全与密钥

### 11.1 安全审计

```bash
# 安全审计
openclaw security audit
```

### 11.2 密钥管理

```bash
# 重新加载密钥
openclaw secrets reload

# 迁移密钥
openclaw secrets migrate
```

---

## 12. 日志与监控

### 12.1 日志查看

```bash
# 查看日志
openclaw logs

# 实时日志
openclaw logs --follow
```

### 12.2 健康检查

```bash
# 健康状态
openclaw health

# 详细状态
openclaw status
```

### 12.3 系统事件

```bash
# 查看系统事件
openclaw system event

# 心跳配置
openclaw system heartbeat last
openclaw system heartbeat enable
openclaw system heartbeat disable

# 在线状态
openclaw system presence
```

---

## 13. 快捷命令

### 13.1 常用快捷命令

```bash
# 快速配置（交互式）
openclaw configure

# 快速检查健康
openclaw doctor

# 快速备份
openclaw backup create

# 快速重启网关
openclaw gateway restart

# 快速查看状态
openclaw status
```

### 13.2 快捷别名

```bash
# dashboard
openclaw dashboard

# TUI 界面
openclaw tui

# 文档
openclaw docs

# 更新
openclaw update
openclaw update --check
```

---

## 附录

### A. 配置路径速查

| 路径 | 说明 |
|------|------|
| `agents.defaults.workspace` | 默认工作区 |
| `agents.defaults.model.primary` | 默认模型 |
| `gateway.port` | 网关端口 |
| `gateway.bind` | 绑定模式 |
| `channels.<type>.enabled` | 通道启用状态 |
| `skills.allowBundled` | 允许的技能列表 |
| `plugins.allow` | 允许的插件列表 |

### B. 环境变量

| 变量 | 说明 |
|------|------|
| `OPENCLAW_GATEWAY_TOKEN` | 网关 Token |
| `OPENCLAW_GATEWAY_URL` | 网关 URL |
| `NO_COLOR` | 禁用颜色 |

### C. 配置文件位置

- **主配置**: `~/.openclaw/openclaw.json`
- **开发配置**: `~/.openclaw-dev/openclaw.json`
- **密钥**: `~/.openclaw/secrets.json`

---

## 更新日志

- **2026-03-18**: 初始版本，包含 OpenClaw 核心命令汇总

---

*由 neuroXY 整理 🦐*
