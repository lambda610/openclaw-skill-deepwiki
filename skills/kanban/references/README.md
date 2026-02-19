# Kanban 异步任务系统

> 轻量级异步任务管理方法，配合 OpenClaw 使用

---

## 核心理念

- **异步协作**：人不需要实时响应，agent 可自主推进
- **多任务并行**：提高多项任务执行效率
- **状态可见**：任务进度清晰可查

---

## 文件结构

```
~/todo/                    # 任务数据目录
├── inbox.md              # 任务入口
├── active.md             # 执行中（≤10条）
├── backlog.md            # 待处理队列
├── projects/             # 按项目分类
└── archive/              # 已完成，按月归档
```

---

## 触发机制

### 1. Cron 定时检查（推荐）
```
每 1 小时检查一次 ~/todo/inbox.md
```

### 2. 事件触发
```
在任务汇报频道被 @ 时触发检查
```

---

## 执行流程

### 1. 检查 inbox
- 读取 inbox.md
- 解析新任务（优先级、项目）
- 分配任务 ID

### 2. 分发任务
- P0 → 立即处理，进 active
- P1 → 进 active 或 backlog
- P2/P3/P4 → 进 backlog

### 3. 执行任务
- 检查前置依赖（depends-on）
- 更新状态：in_progress
- 完成后标记 done，移入 archive
- 卡住标记 blocked

### 4. 汇报
- 做完推送到通知频道
- 定期汇总进度

---

## 状态流转

```
pending → in_progress → done
              ↓
           blocked → (等待确认) → in_progress
```

---

## 任务格式

### inbox 格式
```
[P优先级] [项目名] 任务描述
```

### active 格式
```markdown
- [in_progress] T001: [P1] [project] 任务描述
  - owner: agent-name
  - depends-on: T000
  - updated: YYYY-MM-DD
```

---

## 优先级

- P0: 紧急
- P1: 重要
- P2: 常规
- P3: 低优
- P4: 再说

---

## 配置说明

### 通知频道
根据你的平台选择：
- Discord: 使用 channel ID
- Telegram: 使用 chat ID

### Cron 频率
根据需要调整：
- 每小时: `every 1h`
- 每 30 分钟: `every 30m`

---

## 安装配置

1. 创建任务目录：
```bash
mkdir -p ~/todo/{projects,archive}
```

2. 创建基础文件（参考 templates/）

3. 配置 Cron（参考下方的 OpenClaw 配置示例）

---

## OpenClaw Cron 配置示例

```bash
# 每小时检查 inbox
openclaw cron add \
  --name "todo-inbox-check" \
  --every "1h" \
  --message "1. 读取 ~/todo/README.md 了解执行规则 2. 读取 ~/todo/inbox.md 检查是否有新任务 3. 按规则处理任务并汇报进度到 <通知频道ID>" \
  --channel <discord/telegram> \
  --to "<通知频道ID>" \
  --announce \
  --expect-final
```

---

## 进阶用法

### 按项目分类
在 `projects/` 目录下创建项目文件：
```
projects/
├── skill-factory.md
└── stock-research.md
```

### Delta 更新
不每次重写全量，用 delta 格式标记变更：
```markdown
## 2026-02-19 Updates

### ADDED
- T004: [P1] 新任务

### COMPLETED
- T001, T002
```

---

## 相关资源

- OpenClaw 文档: https://docs.openclaw.ai
- Skill: `kanban` (在公开 skills 仓库)
