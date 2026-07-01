# Thinking Level Map 详解

`thinkingLevelMap` 用于将 Pi 的 thinking level 映射到 Provider 特定值，并标记不支持的 level。**仅当 `reasoning: true` 时有效。**

## Pi Thinking Levels

Pi 支持 6 个 level：`off`、`minimal`、`low`、`medium`、`high`、`xhigh`。

## 值语义

| 值 | 含义 |
|----|------|
| 省略（不在 map 中） | 该 level 受支持，使用 Provider 默认映射 |
| 字符串 | 该 level 受支持，此值发送给 Provider |
| `null` | 该 level 不受支持，在 UI 中隐藏/跳过/固定到最近值 |

## 示例

### 仅支持 off / high / max 的模型

```json
{
  "id": "deepseek-v4-pro",
  "reasoning": true,
  "thinkingLevelMap": {
    "minimal": null,
    "low": null,
    "medium": null,
    "high": "high",
    "xhigh": "max"
  }
}
```

`off` 未指定 → 使用默认映射。`minimal`/`low`/`medium` 为 `null` → 不可用。`high` → 发送 `"high"`，`xhigh` → 发送 `"max"`。

### 不可禁用 thinking 的模型

```json
{
  "id": "always-thinking-model",
  "reasoning": true,
  "thinkingLevelMap": {
    "off": null
  }
}
```

`off` 为 `null` → thinking 不可关闭。其余 level 使用默认映射。

### 映射到自定义字符串

```json
{
  "id": "custom-model",
  "reasoning": true,
  "thinkingLevelMap": {
    "off": "disabled",
    "low": "low-effort",
    "medium": "balanced",
    "high": "high-effort",
    "xhigh": "maximum"
  }
}
```

## 迁移提示

旧版使用 `compat.reasoningEffortMap`，应迁移至模型级别的 `thinkingLevelMap`。
