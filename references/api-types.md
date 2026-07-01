# 支持的 API 类型

在 Provider 级别或模型级别设置 `api` 字段。

| API | 说明 | 典型用途 |
|-----|------|----------|
| `openai-completions` | OpenAI Chat Completions | 最兼容的选择，适用于大多数第三方 API |
| `openai-responses` | OpenAI Responses API | OpenAI 新版 API |
| `anthropic-messages` | Anthropic Messages API | Anthropic 及兼容代理 |
| `google-generative-ai` | Google Generative AI | Google AI Studio / Vertex AI |

## 选择指南

| 场景 | 推荐 API |
|------|----------|
| Ollama / LM Studio / vLLM / SGLang | `openai-completions` |
| 自定义 OpenAI 兼容代理 | `openai-completions` |
| Anthropic 兼容代理 | `anthropic-messages` |
| Google AI Studio | `google-generative-ai` |
| OpenRouter | `openai-completions` |

## 注意事项

- `openai-completions` + 本地服务器的常见配置：
  ```json
  {
    "baseUrl": "http://localhost:11434/v1",
    "api": "openai-completions",
    "apiKey": "ollama"
  }
  ```
  `apiKey` 为占位值，因为 Ollama 忽略它。

- `google-generative-ai` 添加自定义模型时**必须提供 `baseUrl`**。
- 模型级别的 `api` 会覆盖 Provider 级别设置。
