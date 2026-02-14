# prompt-optimizer

AI 提示词优化工具，基于 [linshenkx/prompt-optimizer](https://github.com/linshenkx/prompt-optimizer) 项目内置模板。

## 功能

| 功能 | 模板数 | 说明 |
|------|--------|------|
| 用户提示词优化 | 6 | 基础/规划/专业 + 上下文版 |
| 系统提示词优化 | 6 | 通用/分析/格式 + 上下文版 |
| 迭代改进 | 3 | 通用/上下文/图像 |
| 文生图优化 | 5 | 通用/中文/摄影/创意/JSON |
| 图生图优化 | 3 | 通用/文案替换/JSON |
| 提示词评估 | 20+ | 原始/优化后/对比/直接/迭代 |
| 变量处理 | 2 | 提取/值生成 |

## 快速开始

### 用户提示词优化

```
优化：帮我写个文章
规划：帮我做一个旅行计划
专业优化：帮我处理数据
```

### 系统提示词优化

```
系统提示词：你是一个助手
```

### 图像优化

```
/image 一只猫
图生图：把背景换成夜景
```

### 评估对比

```
评估：
原始提示词：帮我写个文章
优化后提示词：请写一篇关于AI的文章...
```

## 触发关键词

| 功能 | 关键词 |
|------|--------|
| 用户优化 | `优化：`, `基础优化：`, `规划：`, `专业优化：` |
| 系统优化 | `系统提示词：`, `/system`, `格式化：`, `分析型：` |
| 迭代 | `迭代：`, `改进：` |
| 文生图 | `/image`, `图像：` |
| 图生图 | `图生图：`, `编辑：` |
| 评估 | `评估：`, `对比：` |
| 变量 | `提取变量：`, `生成变量：` |

## 目录结构

```
prompt-optimizer/
├── SKILL.md              # 主文件
├── workflows/            # OpenClaw 命令
│   ├── optimize.yaml
│   ├── system.yaml
│   ├── image.yaml
│   └── evaluate.yaml
└── references/           # 详细文档
    ├── templates.md      # 完整模板清单
    ├── user-optimize.md  # 用户优化指南
    ├── system-optimize.md # 系统优化指南
    ├── image-optimize.md # 图像优化指南
    ├── iterate.md       # 迭代优化指南
    ├── evaluate.md      # 评估指南
    └── variables.md     # 变量处理指南
```

## 模板来源

所有优化模板来自 prompt-optimizer 项目的内置模板：

- `user-optimize`: 用户提示词优化
- `optimize`: 系统提示词优化  
- `iterate`: 迭代优化
- `image-optimize`: 图像优化
- `evaluation`: 提示词评估
- `variable-*`: 变量处理

详见 [references/templates.md](references/templates.md)

## License

基于 prompt-optimizer 项目，遵循其开源协议。
