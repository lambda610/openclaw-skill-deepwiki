---
name: prompt-optimizer
description: "AI 提示词优化工具。支持：(1) 用户提示词优化（基础/规划/专业），(2) 系统提示词优化（通用/分析/格式），(3) 迭代改进，(4) 文生图/图生图优化，(5) 提示词评估，(6) 变量提取与生成。使用场景：优化模糊提示词、构建系统提示词、改进现有提示词、生成图像提示词、评估提示词质量、提取/生成变量。"
metadata:
  {
    "openclaw": {
      "emoji": "✨",
      "user-invocable": true
    }
  }
---

# Prompt Optimizer

AI 提示词优化工具，基于 prompt-optimizer 项目内置模板。

## 快速开始

### 触发方式

**自动触发** - 描述需求即可：
```
优化：帮我写个文章
系统提示词：你是一个助手
/image 一只猫
评估：
原始提示词：xxx
优化后提示词：xxx
```

**命令触发** (OpenClaw)：
```
/prompt-optimizer user <提示词>
/prompt-optimizer system <提示词>
/prompt-optimizer iterate <提示词>
/prompt-optimizer image <描述>
/prompt-optimizer evaluate
```

## 功能概览

| 功能 | 模板数 | 说明 |
|------|--------|------|
| 用户提示词优化 | 6 | 基础/规划/专业 + 上下文版 |
| 系统提示词优化 | 6 | 通用/分析/格式 + 上下文版 |
| 迭代改进 | 3 | 通用/上下文/图像 |
| 文生图优化 | 5 | 通用/中文/摄影/创意/JSON |
| 图生图优化 | 3 | 通用/文案替换/JSON |
| 提示词评估 | 20+ | 原始/优化后/对比/直接/迭代 |
| 变量处理 | 2 | 提取/值生成 |

详细模板列表见 [references/templates.md](references/templates.md)

## 核心模板

### 用户提示词优化

详见 [references/user-optimize.md](references/user-optimize.md)

### 系统提示词优化

详见 [references/system-optimize.md](references/system-optimize.md)

### 图像优化

详见 [references/image-optimize.md](references/image-optimize.md)

### 迭代优化

详见 [references/iterate.md](references/iterate.md)

### 提示词评估

详见 [references/evaluate.md](references/evaluate.md)

### 变量处理

详见 [references/variables.md](references/variables.md)
