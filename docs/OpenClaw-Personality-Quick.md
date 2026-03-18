# OpenClaw 人格配置速查

> 一页速查

---

## 核心文件

| 文件 | 作用 |
|------|------|
| SOUL.md | 你是谁（人格） |
| USER.md | 主人是谁 |
| IDENTITY.md | 公开身份 |
| AGENTS.md | 工作区规范 |
| HEARTBEAT.md | 定时任务 |
| MEMORY.md | 长期记忆 |

---

## 加载顺序

```
每次会话启动：
1. SOUL.md      → 你的人格
2. USER.md      → 主人信息
3. memory/今天  → 今日事项
4. memory/昨天  → 昨日事项
5. MEMORY.md   → 长期记忆（仅主会话）
```

---

## 目录结构

```
工作区/
├── SOUL.md
├── USER.md
├── IDENTITY.md
├── AGENTS.md
├── HEARTBEAT.md
├── TOOLS.md
├── MEMORY.md
└── memory/
    └── YYYY-MM-DD.md
```

---

## 快速配置

### 1. 创建 SOUL.md

```markdown
# SOUL.md - Who You Are

## Core Truths
- 简洁直接
- 可以有观点
- 自己先尝试

## Boundaries
- 隐私优先
- 不确定先问

## Vibe
像朋友一样，不要 corporate
```

### 2. 创建 USER.md

```markdown
# USER.md

- **Name:** xxx
- **称呼:** 主人
- **目标:** xxx
```

### 3. 创建 AGENTS.md

```markdown
# AGENTS.md

## Every Session
1. Read SOUL.md
2. Read USER.md
3. Read memory/今天
4. Read memory/昨天
```

---

## 常用命令

```bash
# 查看工作区
openclaw config get agents.defaults.workspace

# 重启网关生效
openclaw gateway restart
```

---

*完整版：OpenClaw-Personality-Guide.md 🦐*
