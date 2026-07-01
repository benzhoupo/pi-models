# Provider 配置字段详解

## 字段速查

| 字段 | 必需 | 说明 |
|------|------|------|
| `baseUrl` | 自定义 Provider 必需 | API 端点 URL |
| `api` | 自定义 Provider 必需 | API 类型，见 `api-types.md` |
| `apiKey` | 否 | API Key；可由 `/login`、`auth.json` 或 CLI `--api-key` 提供 |
| `headers` | 否 | 自定义请求头（支持值解析） |
| `authHeader` | 否 | 设为 `true` 自动添加 `Authorization: Bearer <apiKey>` |
| `models` | 否 | 模型配置数组 |
| `modelOverrides` | 否 | 内置模型的逐模型覆盖 |
| `compat` | 否 | Provider 级别兼容性设置，会被模型级别的 `compat` 合并覆盖 |

## apiKey 与 headers 的值解析

支持三种格式：

- **Shell 命令**：`"!command"` 执行命令，使用 stdout
  ```json
  "apiKey": "!op read 'op://vault/item/credential'"
  ```

- **环境变量**：`"$ENV_VAR"` 或 `"${ENV_VAR}"`
  ```json
  "apiKey": "$MY_API_KEY"
  ```

- **字面值**：直接使用。纯大写字符串是字面量，用 `$` 前缀表示环境变量。
  ```json
  "apiKey": "sk-abc123"
  ```

- **转义**：`"$$"` → 字面 `$`；`"$!"` → 字面 `!`

## modelOverrides

只覆盖特定内置模型，不需替换完整 `models` 列表。支持字段：`name`、`reasoning`、`input`、`cost`（部分）、`contextWindow`、`maxTokens`、`headers`、`compat`。

```json
{
  "providers": {
    "openrouter": {
      "modelOverrides": {
        "anthropic/claude-sonnet-4": {
          "name": "Claude Sonnet 4 (Bedrock Route)",
          "compat": {
            "openRouterRouting": { "only": ["amazon-bedrock"] }
          }
        }
      }
    }
  }
}
```

## 自定义请求头

```json
{
  "providers": {
    "custom-proxy": {
      "baseUrl": "https://proxy.example.com/v1",
      "apiKey": "$MY_API_KEY",
      "api": "anthropic-messages",
      "headers": {
        "x-portkey-api-key": "$PORTKEY_API_KEY",
        "x-secret": "!op read 'op://vault/item/secret'"
      }
    }
  }
}
```

## 认证说明

- `apiKey` 不是加载文件的必要条件
- 未配置认证时模型仍然加载，但在 `/model` 和 `--list-models` 中不可用
- 认证方式优先级：`/login`/`auth.json` > CLI `--api-key` > Provider 的 `apiKey`
- 对于无密钥的本地服务器（如 Ollama），填写占位值并通过 `/login` 或 `--api-key` 传任意值
