# OpenClaw Skills 速查表

> 一页速查

---

## 安装/卸载

```bash
# 安装 ClawHub
npm i -g clawhub

# 搜索
clawhub search <关键词>

# 安装
clawhub install <skill-name>

# 更新
clawhub update <skill>
clawhub update --all

# 列出已安装
clawhub list

# 发布
clawhub publish ./my-skill --slug <slug> --name "Name" --version 1.0
```

---

## 本地管理

```bash
# 列出 Skills
openclaw skills list

# 查看详情
openclaw skills info <name>

# 检查
openclaw skills check
```

---

## SKILL.md 结构

```markdown
---
name: skill-name
description: 技能描述（触发条件）
triggers:
  - keyword1
  - keyword2
metadata:
  openclaw:
    requires:
      bins: ["python3"]
      env: ["API_KEY"]
---

# 标题

## 何时使用

...

## 使用方法

...
```

---

## 配置

```json
{
  "skills": {
    "allowBundled": ["skill1", "skill2"],
    "entries": {
      "my-skill": {
        "enabled": true,
        "env": {
          "API_KEY": "xxx"
        }
      }
    }
  }
}
```

---

## 目录结构

```
skill/
├── SKILL.md
├── scripts/
│   └── main.py
├── references/
└── assets/
```

---

## 常用 Skills

| 用途 | Skill |
|------|-------|
| GitHub | github-cli |
| 图像生成 | minimax-image-gen |
| 安全审计 | security-auditor |
| 编程 | coding-agent |
| 创建技能 | skill-creator |
| 天气 | weather |

---

*完整版：OpenClaw-Skills-Guide.md 🦐*
