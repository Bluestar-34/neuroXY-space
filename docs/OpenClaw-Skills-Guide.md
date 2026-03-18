# OpenClaw Skills 完全指南 | 打造你的超级龙虾

> 整理日期：2026-03-18
> 版本：2026.3+
> 作者：neuroXY

---

## 目录

1. [什么是 Skills？](#1-什么是-skills)
2. [Skills 架构解析](#2-skills-架构解析)
3. [Skills 存放位置](#3-skills-存放位置)
4. [ClawHub 技能市场](#4-clawhub-技能市场)
5. [安装与管理 Skills](#5-安装与管理-skills)
6. [创建自定义 Skill](#6-创建自定义-skill)
7. [SKILL.md 完全指南](#7-skillmd-完全指南)
8. [技能安全与权限](#8-技能安全与权限)
9. [最佳实践](#9-最佳实践)
10. [实战案例](#10-实战案例)

---

## 1. 什么是 Skills？

### 1.1 定义

**Skills（技能）** 是 OpenClaw 赋予 AI 能力的方式。每个 Skill 是一个目录，包含：
- `SKILL.md` - 定义技能元数据和使用说明
- `scripts/` - 可选的可执行脚本
- `references/` - 参考文档
- `assets/` - 资源文件

### 1.2 Skills 的作用

```
用户请求 → Agent 判断需要 → 加载对应 Skill → 执行任务
```

### 1.3 Skills vs Plugins

| 特性 | Skills | Plugins |
|------|--------|---------|
| 形式 | Markdown 配置 | Node.js 代码 |
| 复杂度 | 简单 | 复杂 |
| 用途 | 教学/工作流 | 深度集成 |
| 示例 | 图像生成、GitHub 操作 | 飞书、Discord 通道 |

---

## 2. Skills 架构解析

### 2.1 核心概念

```
SKILL.md
├── YAML Frontmatter (元数据)
│   ├── name          - 技能名称
│   ├── description   - 描述（触发条件）
│   └── metadata      - OpenClaw 配置
│
└── Markdown Body (使用说明)
    ├── 何时使用
    ├── 如何使用
    └── 示例代码
```

### 2.2 触发机制

Skills 通过 **description** 和 **triggers** 触发：

```markdown
---
name: github-cli
description: Manage GitHub repositories, issues, and pull requests
triggers:
  - github
  - repository
  - issue
  - pull request
---
```

当用户提到这些关键词时，AI 会自动加载对应技能。

---

## 3. Skills 存放位置

### 3.1 三个层级（优先级从高到低）

| 位置 | 路径 | 说明 |
|------|------|------|
| **Workspace** | `<workspace>/skills` | 当前工作区 |
| **Managed/Local** | `~/.openclaw/skills` | 本地共享 |
| **Bundled** | `npm包/skills` | 内置技能 |

### 3.2 优先级规则

```
workspace/skills (最高)
    ↓
~/.openclaw/skills
    ↓
内置 skills (最低)
```

同名技能会按优先级覆盖。

### 3.3 自定义目录

在配置文件中添加：

```json
{
  "skills": {
    "load": {
      "extraDirs": ["~/Projects/agent-skills"]
    }
  }
}
```

---

## 4. ClawHub 技能市场

### 4.1 什么是 ClawHub？

ClawHub 是 OpenClaw 官方的 Skills 市场和共享平台。

**官网**：https://clawhub.com

### 4.2 常用命令

```bash
# 安装 ClawHub CLI
npm i -g clawhub

# 搜索技能
clawhub search <关键词>

# 安装技能（到当前目录的 skills/）
clawhub install <slug>

# 更新技能
clawhub update <slug>
clawhub update --all

# 列出已安装
clawhub list

# 发布技能
clawhub publish ./my-skill --slug <slug> --name "My Skill" --version 1.0.0
```

### 4.3 常用 Skills 推荐

| 分类 | Skill | 说明 |
|------|-------|------|
| **AI 图像** | minimax-image-gen | MiniMax 图像生成 |
| **开发** | github-cli | GitHub 操作 |
| **开发** | coding-agent | AI 编程代理 |
| **安全** | security-auditor | 代码安全审计 |
| **效率** | weather | 天气查询 |
| **效率** | obsidian | Obsidian 集成 |

---

## 5. 安装与管理 Skills

### 5.1 查看 Skills

```bash
# 列出所有可用 Skills
openclaw skills list

# 查看 Skill 详情
openclaw skills info <skill-name>

# 检查 Skills 状态
openclaw skills check
```

### 5.2 配置 Skills

在 `~/.openclaw/openclaw.json` 中：

```json
{
  "skills": {
    "allowBundled": ["github", "weather", "skill-creator"],
    "entries": {
      "my-skill": {
        "enabled": true,
        "env": {
          "MY_API_KEY": "your-key"
        }
      }
    }
  }
}
```

### 5.3 环境变量注入

```json
{
  "skills": {
    "entries": {
      "minimax-image-gen": {
        "env": {
          "MINIMAX_API_KEY": "your-key"
        }
      }
    }
  }
}
```

---

## 6. 创建自定义 Skill

### 6.1 目录结构

```
my-skill/
├── SKILL.md              # 必需
├── scripts/              # 可选
│   └── main.py
├── references/           # 可选
│   └── guide.md
└── assets/              # 可选
    └── template.json
```

### 6.2 最简示例

```markdown
---
name: hello_world
description: A simple greeting skill
---

# Hello World Skill

Use the echo tool to greet the user with a friendly message.

## Usage

When user says "hello" or "hi", respond with a warm greeting.
```

### 6.3 使用 skill-creator

安装 skill-creator 来帮助你创建：

```bash
clawhub install skill-creator
```

然后告诉 AI："创建一个 xxx skill"

---

## 7. SKILL.md 完全指南

### 7.1 必须字段

```markdown
---
name: skill-name
description: 技能描述，告诉 AI 什么时候使用这个技能
---
```

### 7.2 完整元数据

```markdown
---
name: my-skill
description: 技能描述
triggers:
  - 触发关键词1
  - 触发关键词2
role: specialist|generalist
scope: 作用域
output-format: structured|plain
metadata:
  openclaw:
    emoji: "🎨"
    requires:
      bins: ["python3", "git"]
      env: ["API_KEY"]
      config: ["feature.enabled"]
    primaryEnv: API_KEY
    install:
      - id: python-installed
        kind: note
        label: "Python 3 is required"
---
```

### 7.3 字段详解

| 字段 | 说明 | 示例 |
|------|------|------|
| `name` | 技能唯一标识 | `github-cli` |
| `description` | 描述（AI 判断是否使用） | `GitHub 仓库管理` |
| `triggers` | 触发关键词列表 | `["github", "issue"]` |
| `role` | 角色类型 | `specialist` |
| `metadata.openclaw.requires.bins` | 需要的二进制命令 | `["python3"]` |
| `metadata.openclaw.requires.env` | 需要的环境变量 | `["API_KEY"]` |
| `metadata.openclaw.primaryEnv` | 主要 API Key | `OPENAI_API_KEY` |

### 7.4 条件触发

```markdown
---
name: docker-skill
metadata:
  openclaw:
    requires:
      bins: ["docker"]
---
```

只有当 `docker` 命令存在时才会加载。

---

## 8. 技能安全与权限

### 8.1 安全原则

- ⚠️ **第三方 Skills 如同未知代码**
- 🔒 **敏感操作使用沙箱运行**
- 🔑 **API Key 不写入 Skill 代码**

### 8.2 限制 Skills

```json
{
  "skills": {
    "allowBundled": ["github", "weather", "obsidian"]
  }
}
```

只允许指定技能。

### 8.3 禁用特定 Skill

```json
{
  "skills": {
    "entries": {
      "dangerous-skill": {
        "enabled": false
      }
    }
  }
}
```

### 8.4 沙箱运行

对于不安全输入，使用沙箱模式：

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "docker"
      }
    }
  }
}
```

---

## 9. 最佳实践

### 9.1 SKILL.md 写作原则

1. **简洁优先** - AI 已经很聪明，只需告诉它不知道的
2. **示例胜过解释** - 代码示例比文字说明更清晰
3. **适度自由度** - 根据任务复杂度调整指导程度

### 9.2 目录组织

```
skill-name/
├── SKILL.md
├── scripts/
│   └── main.py        # 主脚本
├── references/
│   └── api.md         # API 文档
└── assets/
    └── template.json  # 模板
```

### 9.3 脚本最佳实践

- 使用 Python/Bash 等通用语言
- 添加 `--help` 参数
- 返回结构化输出（JSON）
- 错误处理完善

---

## 10. 实战案例

### 10.1 案例：GitHub 操作 Skill

```markdown
---
name: github-manager
description: Manage GitHub repositories, issues, and PRs
triggers:
  - github
  - repository
  - create repo
  - create issue
metadata:
  openclaw:
    requires:
      bins: ["gh"]
---

# GitHub Manager

Use GitHub CLI (gh) to manage repositories.

## Create Repository

```bash
gh repo create <name> --public --description "<desc>"
```

## Create Issue

```bash
gh issue create --title "<title>" --body "<body>"
```
```

### 10.2 案例：自定义 API 调用 Skill

```markdown
---
name: my-api-caller
description: Call custom API endpoints
triggers:
  - api
  - call
metadata:
  openclaw:
    requires:
      env: ["MY_API_KEY"]
---

# My API Caller

Call custom API with authentication.

## Usage

```bash
curl -H "Authorization: Bearer $MY_API_KEY" \
  "https://api.example.com/endpoint"
```

## Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /users | List users |
| POST | /users | Create user |
```

---

## 附录

### A. Skills 命令速查

```bash
# 列出 Skills
openclaw skills list

# 查看详情
openclaw skills info <name>

# 检查状态
openclaw skills check

# ClawHub
clawhub search <kw>
clawhub install <slug>
clawhub update <slug>
clawhub publish ./skill --slug <s> --name "N" --version 1.0
```

### B. 配置路径

| 配置项 | 路径 |
|--------|------|
| Skills 列表 | `skills.allowBundled` |
| 技能配置 | `skills.entries.<name>` |
| 额外目录 | `skills.load.extraDirs` |
| 安装偏好 | `skills.install.nodeManager` |

### C. 常用触发词

- `github` - GitHub 操作
- `image` - 图像生成
- `weather` - 天气查询
- `search` - 搜索
- `code` - 编程
- `file` - 文件操作

---

## 更新日志

- **2026-03-18**: 初始版本

---

*由 neuroXY 整理 🦐*
