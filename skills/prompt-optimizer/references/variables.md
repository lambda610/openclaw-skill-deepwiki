# 变量处理

从提示词中提取变量，或根据上下文生成变量值。

## 变量提取 (Variable Extraction)

### 功能

从提示词中自动识别和提取变量占位符。

### 触发方式

```
提取变量：
请生成一份关于{topic}的报告，字数{length}字
```

### 输出格式

```json
{
  "variables": [
    {
      "name": "topic",
      "description": "报告主题",
      "example": "人工智能",
      "required": true
    },
    {
      "name": "length",
      "description": "字数要求",
      "example": "1000",
      "required": true
    }
  ]
}
```

### 使用场景

1. **模板设计**: 从示例提示词中提取变量
2. **表单生成**: 自动生成输入表单
3. **变量清单**: 列出提示词中所有变量

---

## 变量值生成 (Variable Value Generation)

### 功能

根据提示词上下文推测变量的可能值。

### 触发方式

```
生成变量值：
模板：{name}是一个{profession}，擅长{skill}
上下文：科技行业，技术写作
```

### 输出格式

```json
{
  "suggestions": [
    {
      "name": "name",
      "value": "张明",
      "reason": "常见的中国名字"
    },
    {
      "name": "profession", 
      "value": "技术作家",
      "reason": "符合科技行业上下文"
    },
    {
      "name": "skill",
      "value": "技术文档撰写",
      "reason": "与技术写作相关"
    }
  ]
}
```

### 使用场景

1. **默认值建议**: 为变量推荐合理默认值
2. **测试数据**: 生成测试用例
3. **快速填充**: 一键填充表单

---

## 使用示例

### 示例1: 变量提取

**输入**:
```
提取变量：
请为{product}撰写一篇{type}，目标读者是{audience}，字数约{word_count}字
```

**输出**:
```json
{
  "variables": [
    {
      "name": "product",
      "description": "产品名称",
      "example": "智能手表",
      "required": true
    },
    {
      "name": "type",
      "description": "文章类型",
      "example": "评测文章",
      "required": true
    },
    {
      "name": "audience",
      "description": "目标读者",
      "example": "科技爱好者",
      "required": true
    },
    {
      "name": "word_count",
      "description": "字数要求",
      "example": "2000",
      "required": false
    }
  ]
}
```

---

### 示例2: 变量值生成

**输入**:
```
生成变量值：
模板：你是{role}，负责{task}
上下文：软件开发团队，技术文档撰写
```

**输出**:
```json
{
  "suggestions": [
    {
      "name": "role",
      "value": "技术文档工程师",
      "reason": "符合软件开发团队背景"
    },
    {
      "name": "task",
      "value": "编写API文档",
      "reason": "技术文档撰写的具体形式"
    }
  ]
}
```

---

## 触发关键词

| 功能 | 关键词 |
|------|--------|
| 变量提取 | `提取变量：`, `变量提取：`, `extract variables` |
| 变量生成 | `生成变量：`, `变量值生成：`, `generate variables` |

---

## 对应模板

| 功能 | 模板 |
|------|------|
| 变量提取 | variableExtractionTemplate |
| 变量生成 | variableValueGenerationTemplate |
