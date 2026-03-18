# OpenClaw 实战命令案例

> 基于 blue 大人的实际使用场景

---

## 场景1: 配置 GitHub

```bash
# 1. 检查 gh 是否可用
gh --version

# 2. 登录 GitHub
gh auth login

# 3. 查看仓库
gh repo list
```

---

## 场景2: 配置 Skills

```bash
# 搜索 Skills
clawhub search "github"
clawhub search "image"
clawhub search "security"

# 安装 Skills
clawhub install github-cli
clawhub install security-auditor

# 强制安装（遇到警告时）
clawhub install <skill> --force
```

---

## 场景3: 配置模型

```bash
# 查看当前模型
openclaw models status

# 列出可用模型
openclaw models list

# 配置 MiniMax（已有）
# 位置：~/.openclaw/openclaw.json -> models.providers.minimax-cn
```

---

## 场景4: 配置通道

```bash
# 配置飞书
openclaw configure --section channels
# 选择 feishu

# 配置 QQ
openclaw configure --section channels
# 选择 qqbot
```

---

## 场景5: 网关管理

```bash
# 查看网关状态
openclaw gateway status

# 重启网关
openclaw gateway restart

# 查看日志
openclaw gateway logs
```

---

## 场景6: 调试问题

```bash
# 健康检查
openclaw doctor

# 详细诊断
openclaw doctor --fix

# 查看日志
openclaw logs --follow
```

---

## 场景7: 备份与迁移

```bash
# 创建备份
openclaw backup create

# 验证备份
openclaw backup verify
```

---

## 场景8: 安全配置

```bash
# 安全审计
openclaw security audit

# 查看配置的安全设置
openclaw config get gateway.auth
openclaw config get gateway.tailscale
```

---

## 场景9: 配置技能白名单

```bash
# 只允许特定 Skills
openclaw config set skills.allowBundled '["github", "skill-creator", "clawhub", "weather"]'

# 禁用特定技能
openclaw config set skills.entries.skill-name.enabled false
```

---

## 场景10: 使用代理/Agent

```bash
# 列出 Agents
openclaw agents list

# 查看会话
openclaw sessions
```

---

## 常用配置路径速查

```bash
# 网关
gateway.port                    # 端口
gateway.bind                    # 绑定模式 (loopback/lan/tailnet)
gateway.auth.mode               # 认证模式

# 模型
models.providers.minimax-cn.apiKey   # MiniMax API Key

# 通道
channels.discord.token          # Discord Token
channels.telegram.token         # Telegram Token
channels.feishu.appId           # 飞书 App ID

# 技能
skills.allowBundled             # 允许的技能
skills.entries                 # 技能配置
```

---

*持续更新中... 🦐*
