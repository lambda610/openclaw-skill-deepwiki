# Kanban 异步任务系统

> 轻量级异步任务管理方法，配合 OpenClaw 使用

**⚠️ 注意**：这是一个轻量级 skill，不包含安装脚本。需要手动配置目录和 cron（见下方安装配置章节）。

---

## 核心理念

- **异步协作**：人不需要实时响应，agent 可自主推进
- **多任务并行**：提高多项任务执行效率
- **状态可见**：任务进度清晰可查

---

## 文件结构

```
~/kanban/                    # 任务数据目录
├── inbox.md                 # 任务入口
├── active.md                # 执行中（≤10条）
├── backlog.md               # 待处理队列
├── projects/                # 按项目分类的参考文档
├── categories/              # 按类别分类的参考文档
├── tags/                    # 按标签分类的参考文档
└── archive/                 # 已完成，按月归档
```

---

## 触发机制

### 1. Cron 定时检查（推荐）
```
每 1 小时检查一次 ~/kanban/inbox.md
```

### 2. 事件触发
```
在任务汇报频道被 @ 时触发检查
```

---

## 执行流程（完整版）

### 步骤 1：检查 inbox
- 读取 inbox.md
- 解析新任务（优先级、项目、类别、标签）
- 分配任务 ID（T001, T002...）

### 步骤 2：分发到 backlog
```
P0 → 立即进入 active
P1 → 进入 active（如果有空位）或 backlog 头部
P2 → 进入 backlog
P3/P4 → 进入 backlog 尾部
```

**Active 上限控制**：active.md 最多 10 条任务，超过时新任务排队等待

### 步骤 3：执行任务
- 从 active.md 选取最高优先级任务
- 检查前置依赖（depends-on）是否满足
- 更新状态为 in_progress
- 开始执行

### 步骤 4：处理阻塞
- 如果任务缺少信息无法继续，标记 blocked
- 记录缺少什么信息，推送给人确认
- 人补充后，状态改回 in_progress 继续执行

### 步骤 5：完成与归档
- 任务完成后标记 done
- 移入 archive/ 按月份归档

### 步骤 6：恢复机制
- 检查 active.md 中是否有 in_progress 状态但超过 N 小时未更新的任务
- 尝试恢复执行或标记 blocked

---

## Human 介入流程

当任务执行过程中需要人补充信息时：

1. **标记 blocked**：状态改为 `blocked`，记录需要的信息
2. **推送通知**：告诉人类需要补充什么
3. **等待补充**：人在任务描述后追加信息，或在频道回复
4. **继续执行**：收到补充后改回 in_progress

示例：
```markdown
- [blocked] T001: [P1] [project:ai] 分析 AI 芯片板块
  - 缺少信息: 需要你提供具体关注哪几家公司的财务数据
  - updated: 2026-02-19
```

---

## 状态流转

```
pending → in_progress → done
              ↓
           blocked → (人补充信息) → in_progress
```

---

## 任务格式

### inbox 格式
```
[P优先级] [project:项目名] [category:类别] [tag:标签] 任务描述
```

### active 格式
```markdown
- [in_progress] T001: [P1] [project:ai] [category:research] 任务描述
  - owner: agent-name
  - depends-on: T000
  - blocked-reason: (如果有)
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

## Delta 格式说明

> 此格式借鉴自 OpenSpec，用于增量更新避免冲突

不每次重写全量文件，而是在文件末尾追加变更记录：

```markdown
## 2026-02-19 Updates

### ADDED
- T004: [P1] [project:ai] 新任务描述

### COMPLETED
- T001, T002

### BLOCKED
- T003: 等待你补充 xx 信息
```

**优点**：
- 避免多版本冲突
- 保留变更历史
- 文件不会随时间膨胀

---

## 元数据目录

### projects/ — 项目参考文档
```
~/kanban/projects/
├── ai.md           # AI 项目通用背景
├── skill-factory.md
└── stock-research.md
```

### categories/ — 类别参考文档
```
~/kanban/categories/
├── research.md     # 研究类任务通用指南
├── development.md # 开发类任务通用指南
└── ops.md         # 运维类任务通用指南
```

### tags/ — 标签参考文档
```
~/kanban/tags/
├── urgent.md      # 紧急任务处理原则
└── blocked.md     # 阻塞处理流程
```

**使用方式**：执行任务时，agent 根据任务的 metadata 自动查阅对应文件

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

## 安装配置（手动）

### 步骤 1：创建本地任务目录

```bash
mkdir -p ~/kanban/{projects,categories,tags,archive}
```

### 步骤 2：创建基础文件

从 skill 的 templates/ 复制模板文件到 ~/kanban/：

```bash
# 复制模板文件
cp ~/projects/public-skills/skills/kanban/references/templates/inbox.md ~/kanban/
cp ~/projects/public-skills/skills/kanban/references/templates/active.md ~/kanban/
cp ~/projects/public-skills/skills/kanban/references/templates/backlog.md ~/kanban/

# 创建 archive 说明
echo "# Archive 归档" > ~/kanban/archive/README.md

# 创建元数据目录说明
echo "# Projects 项目参考" > ~/kanban/projects/README.md
echo "# Categories 类别参考" > ~/kanban/categories/README.md
echo "# Tags 标签参考" > ~/kanban/tags/README.md
```

### 步骤 3：配置 Cron

```bash
# 每小时检查 inbox
openclaw cron add \
  --name "kanban-inbox-check" \
  --every "1h" \
  --message "1. 读取 ~/projects/public-skills/skills/kanban/references/GUIDE.md 了解执行规则 2. 读取 ~/kanban/inbox.md 检查是否有新任务 3. 按规则处理并汇报到 <你的Channel ID>" \
  --channel <discord/telegram> \
  --to "<你的Channel ID>" \
  --announce \
  --expect-final
```

### 步骤 4：通知频道

- 创建一个专属频道用于接收任务汇报（如 #kanban）
- 将频道 ID 填入上方配置

---

## 验证安装

配置完成后，可以手动触发一次测试：

```bash
# 往 inbox 添加测试任务
echo "[P2] 测试任务" >> ~/kanban/inbox.md

# 手动触发 cron
openclaw cron run <job-id>
```

或者直接在频道 @ 你的 agent 说"检查 kanban"。

---

## 相关资源

- OpenClaw 文档: https://docs.openclaw.ai
- Skill: `kanban` (在公开 skills 仓库)
