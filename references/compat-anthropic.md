# Anthropic 兼容性字段

用于 `api: "anthropic-messages"` 的 Provider 或代理。

## 字段速查

| 字段 | 默认值 | 说明 |
|------|--------|------|
| `supportsEagerToolInputStreaming` | `true` | Provider 是否接受 `tools[].eager_input_streaming`。设为 `false` 可省略该字段并使用旧版 `fine-grained-tool-streaming-2025-05-14` beta 头 |
| `supportsLongCacheRetention` | `true` | 支持 `cache_control.ttl: "1h"` 长缓存保留 |
| `sendSessionAffinityHeaders` | 自动检测 | 启用缓存后是否发送 `x-session-affinity` |
| `supportsCacheControlOnTools` | `true` | 是否接受工具定义上的 `cache_control` 标记 |
| `forceAdaptiveThinking` | `false` | 发送 adaptive thinking（`thinking.type: "adaptive"` + `output_config.effort`）。内置 adaptive 模型自动设置 |
| `allowEmptySignature` | `false` | 某些兼容 Provider 发出空签名 thinking 块。仅在需要时设为 `true`；真正 Anthropic 会拒绝空签名 |

## 典型配置

```json
{
  "providers": {
    "anthropic-proxy": {
      "baseUrl": "https://proxy.example.com",
      "api": "anthropic-messages",
      "apiKey": "$ANTHROPIC_PROXY_KEY",
      "compat": {
        "supportsEagerToolInputStreaming": false,
        "supportsLongCacheRetention": true,
        "forceAdaptiveThinking": true,
        "allowEmptySignature": true
      }
    }
  }
}
```

## 使用场景

- `supportsEagerToolInputStreaming: false` — 代理或后端拒绝 `eager_input_streaming` 时
- `forceAdaptiveThinking: true` — 路由到需要 adaptive thinking 的新 Anthropic 模型时
- `allowEmptySignature: true` — 兼容 Provider 在 thinking 签名处理上有差异时（**不要对真正 Anthropic API 使用**）
