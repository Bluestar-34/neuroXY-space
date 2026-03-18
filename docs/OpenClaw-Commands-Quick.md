# OpenClaw 常用命令速查

> 日常使用只需看这一页

---

## 日常高频命令

```bash
# 配置
openclaw configure                      # 交互式配置
openclaw config get <path>              # 查看配置
openclaw config set <path> <value>      # 设置配置

# 网关
openclaw gateway start                  # 启动网关
openclaw gateway stop                    # 停止网关
openclaw gateway restart                 # 重启网关
openclaw gateway status                 # 查看状态

# 状态
openclaw status                         # 完整状态
openclaw health                         # 健康检查
openclaw doctor                         # 诊断问题
```

---

## 技能 Skills

```bash
# 列出技能
openclaw skills list

# ClawHub
clawhub search <关键词>                 # 搜索技能
clawhub install <skill>                 # 安装技能
clawhub update <skill>                  # 更新技能
clawhub list                            # 已安装列表
clawhub publish ./skill --slug xxx      # 发布技能
```

---

## 插件 Plugins

```bash
openclaw plugins list                   # 列出插件
openclaw plugins install <package>      # 安装插件
openclaw plugins enable <name>           # 启用
openclaw plugins disable <name>          # 禁用
```

---

## 模型 Models

```bash
openclaw models list                    # 列出模型
openclaw models status                  # 模型状态
openclaw models set <model>              # 设置默认模型
openclaw models scan                    # 扫描新模型
```

---

## 通道 Channels

```bash
openclaw channels                       # 列出通道
openclaw channels status                # 通道状态
openclaw channels logs                  # 查看日志
```

---

## 定时任务 Cron

```bash
openclaw cron list                      # 列出任务
openclaw cron add ...                   # 添加任务
openclaw cron rm <id>                   # 删除任务
openclaw cron enable <id>               # 启用
openclaw cron disable <id>              # 禁用
```

---

## 会话 Sessions

```bash
openclaw sessions                       # 列出会话
openclaw sessions history               # 历史记录
```

---

## 备份还原

```bash
openclaw backup create                  # 创建备份
openclaw backup verify                  # 验证备份
openclaw reset                          # 重置
```

---

## 快捷查看

```bash
openclaw --help                         # 帮助
openclaw --version                      # 版本
openclaw dashboard                      # 控制面板
openclaw logs                           # 日志
```

---

*有问题看完整版：OpenClaw-Commands.md 🦐*
