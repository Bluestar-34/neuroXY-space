# OpenClaw 人格配置指南

> 通用模板与最佳实践

---

## 目录

1. [人格系统概述](#1-人格系统概述)
2. [核心配置文件](#2-核心配置文件)
3. [配置文件详解](#3-配置文件详解)
4. [记忆系统](#4-记忆系统)
5. [最佳实践](#5-最佳实践)

---

## 1. 人格系统概述

OpenClaw 通过配置文件定义 AI 的人格，包括身份、性格、记忆等。

### 文件架构

```
工作区/
├── SOUL.md         # 核心人格定义
├── USER.md         # 用户信息
├── AGENTS.md       # 工作区规范
├── HEARTBEAT.md    # 定时任务
├── MEMORY.md       # 长期记忆
└── memory/         # 每日记录
```

---

## 2. 核心配置文件

| 文件 | 作用 |
|------|------|
| SOUL.md | 定义 AI 核心人格 |
| USER.md | 了解服务对象 |
| AGENTS.md | 工作区行为规范 |
| HEARTBEAT.md | 周期性任务 |

---

## 3. 配置文件详解

### SOUL.md

```markdown
# SOUL.md - AI Persona

## Core Principles

- Be genuinely helpful
- Have opinions
- Be resourceful

## Boundaries

- Protect privacy
- Ask before external actions

## Vibe

Be concise, friendly, professional
```

### USER.md

```markdown
# USER.md

- **Name:** [Your Name]
- **Background:** [Brief context]

## Goals

- Goal 1
- Goal 2
```

### AGENTS.md

```markdown
# AGENTS.md

## Every Session

1. Read SOUL.md
2. Read USER.md
3. Read memory files

## Memory

- Daily logs: memory/YYYY-MM-DD.md
- Long-term: MEMORY.md
```

---

## 4. 记忆系统

### 短期记忆

`memory/YYYY-MM-DD.md` - 每日记录

### 长期记忆

`MEMORY.md` - 重要事项精选

---

## 5. 最佳实践

1. **简洁原则** - 只记录重要信息
2. **隐私优先** - 不记录敏感信息
3. **定期整理** - 定期清理过期内容
4. **分层存储** - 短期/长期分开

---

*通用模板，请根据实际需求修改*
