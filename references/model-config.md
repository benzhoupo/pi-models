# 模型配置字段详解

## 字段速查

| 字段 | 必需 | 默认值 | 说明 |
|------|------|--------|------|
| `id` | 是 | — | 模型标识符（传递给 API） |
| `name` | 否 | `id` | 人类可读标签，用于模型匹配和次要详情文本。**不替换页脚/状态栏中的 id** |
| `api` | 否 | Provider 的 `api` | 覆盖 Provider 的 API 类型 |
| `reasoning` | 否 | `false` | 是否支持 extended thinking |
| `thinkingLevelMap` | 否 | 省略 | 映射 Pi thinking level 到 Provider 值，`null` 表示不支持某 level。详见 `thinking-level-map.md` |
| `input` | 否 | `["text"]` | 输入类型：`["text"]` 或 `["text", "image"]` |
| `contextWindow` | 否 | `128000` | 上下文窗口大小（Token） |
| `maxTokens` | 否 | `16384` | 最大输出 Token |
| `cost` | 否 | 全零 | `{"input":0,"output":0,"cacheRead":0,"cacheWrite":0}`（每百万 Token） |
| `compat` | 否 | Provider 的 `compat` | 与 Provider 级别的 `compat` 合并，模型级优先 |

## 字段说明

### id

发送给 API 的模型标识符。对于 Ollama：`"llama3.1:8b"`；对于 OpenRouter：`"openrouter/anthropic/claude-sonnet-4"`。

### name

用于 `--model` 模式匹配和列表中的次要详情文本。页脚和主模型列表始终显示 `id`。

### reasoning

设为 `true` 表示模型支持推理/thinking。对 reasoning 模型通常还需配置 `thinkingLevelMap`。

### input

- `["text"]` — 纯文本模型
- `["text", "image"]` — 支持图片输入

### 成本

`cost` 对象包含四个字段（每百万 Token）：
- `input` — 输入成本
- `output` — 输出成本
- `cacheRead` — 缓存读取成本
- `cacheWrite` — 缓存写入成本

全零表示免费。

## 最小模型定义

```json
{ "id": "llama3.1:8b" }
```

仅需 `id`，其余使用默认值。

## 完整模型定义示例

```json
{
  "id": "llama3.1:8b",
  "name": "Llama 3.1 8B (Local)",
  "reasoning": false,
  "input": ["text"],
  "contextWindow": 128000,
  "maxTokens": 32000,
  "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 }
}
```
