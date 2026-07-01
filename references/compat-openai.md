# OpenAI 兼容性字段

用于 `api: "openai-completions"` 或 `api: "openai-responses"` 的 Provider。可设在 Provider 级别（应用于所有模型）或模型级别（覆盖 Provider 值）。

## 字段速查

| 字段 | 说明 |
|------|------|
| `supportsStore` | Provider 支持 `store` 字段 |
| `supportsDeveloperRole` | 使用 `developer` 角色而非 `system`。对不支持的 Provider 设为 `false` |
| `supportsReasoningEffort` | 支持 `reasoning_effort` 参数。不支持的 Provider 设为 `false` |
| `supportsUsageInStreaming` | 支持 `stream_options: { include_usage: true }`（默认：`true`） |
| `maxTokensField` | 使用 `"max_completion_tokens"` 或 `"max_tokens"` |
| `requiresToolResultName` | 在工具结果消息中包含 `name` |
| `requiresAssistantAfterToolResult` | 在工具结果后、用户消息前插入助手消息 |
| `requiresThinkingAsText` | 将 thinking 块转换为纯文本 |
| `requiresReasoningContentOnAssistantMessages` | 启用推理时在所有重放助手消息上包含空 `reasoning_content` |
| `thinkingFormat` | Thinking 参数格式：`"reasoning_effort"`、`"openrouter"`、`"deepseek"`、`"together"`、`"zai"`、`"qwen"`、`"chat-template"`、`"qwen-chat-template"` |
| `chatTemplateKwargs` | `thinkingFormat: "chat-template"` 时使用的 `chat_template_kwargs` 值 |
| `cacheControlFormat` | 设置 `"anthropic"` 启用 Anthropic 风格的 `cache_control` 标记（系统提示、最后工具定义、最后用户/助手文本内容块） |
| `supportsStrictMode` | 在工具定义中包含 `strict` 字段 |
| `supportsLongCacheRetention` | 支持长缓存保留（默认：`true`）。OpenAI：`prompt_cache_retention: "24h"` |
| `openRouterRouting` | OpenRouter Provider 路由偏好对象，按原样发送 |
| `vercelGatewayRouting` | Vercel AI Gateway 路由配置（`only`、`order`） |

## thinkingFormat 详解

| 值 | 行为 |
|----|------|
| `"reasoning_effort"` | 使用 `reasoning_effort` 参数（默认） |
| `"openrouter"` | `reasoning: { effort }` |
| `"deepseek"` | DeepSeek 风格 thinking |
| `"together"` | `reasoning: { enabled }`，启用 `supportsReasoningEffort` 时也使用 `reasoning_effort` |
| `"zai"` | Z.ai 风格 thinking |
| `"qwen"` | 顶级 `enable_thinking` |
| `"qwen-chat-template"` | `chat_template_kwargs.enable_thinking` + `preserve_thinking`，用于本地 Qwen 兼容服务器 |
| `"chat-template"` | 可配置的 `chat_template_kwargs`，配合 `chatTemplateKwargs` 使用 |

## chatTemplateKwargs 用法

使用 `{ "$var": "thinking.enabled" }` 和 `{ "$var": "thinking.effort" }` 作为 Pi 控制的 thinking 变量占位符：

```json
"chatTemplateKwargs": {
  "thinking": { "$var": "thinking.enabled" }
}
```

## OpenRouter 路由示例

```json
"compat": {
  "openRouterRouting": {
    "allow_fallbacks": true,
    "order": ["anthropic", "amazon-bedrock"],
    "only": ["anthropic", "amazon-bedrock"],
    "sort": { "by": "price", "partition": "model" },
    "max_price": { "prompt": 10, "completion": 20 }
  }
}
```

## Vercel AI Gateway 路由示例

```json
"compat": {
  "vercelGatewayRouting": {
    "only": ["fireworks", "novita"],
    "order": ["fireworks", "novita"]
  }
}
```

## 常见组合（Ollama / vLLM 等）

```json
"compat": {
  "supportsDeveloperRole": false,
  "supportsReasoningEffort": false
}
```
