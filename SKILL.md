---
name: pi-models
description: 管理 Pi 编码助手的自定义模型配置（models.json）。当用户请求添加/切换/删除/修改自定义模型或 Provider 时使用，包括「帮我加一个 Ollama 模型」「切换到 DeepSeek」「配置 OpenRouter 路由」「修改 compat 设置」「添加 thinking level map」「列出我的自定义模型」等。覆盖所有 Pi models.json 配置场景：Provider 管理、模型 CRUD、API 类型选择、OpenAI/Anthropic 兼容性设置、thinking 配置、路由策略等。
---

# Pi Models

通过编辑 `~/.pi/agent/models.json`（Windows: `%USERPROFILE%\.pi\agent\models.json`）管理 Pi 的自定义 Provider 和模型。

## 配置文件路径

默认路径（按操作系统）：
- **Linux / macOS**：`~/.pi/agent/models.json`
- **Windows**：`%USERPROFILE%\.pi\agent\models.json`（即 `C:\Users\<用户名>\.pi\agent\models.json`）

如果用户指定了自定义路径，使用该路径；否则使用默认路径。

## 工作流

1. **解析意图**：理解用户要添加/修改/删除/查询什么
2. **读取现有配置**：读取 `models.json`，如文件不存在则从 `{"providers":{}}` 开始
3. **按需查阅参考文档**：需要字段细节时读取对应的 `references/` 文件
4. **展示修改计划**：展示将要修改的 JSON 片段（diff 或完整 new 态），让用户确认
5. **写入文件**：用户确认后写入。如目录不存在则先创建 `~/.pi/agent/`（Windows: `%USERPROFILE%\.pi\agent\`）目录
6. **告知结果**：简要说明做了什么修改

## 字段速查

### Provider 级字段

| 字段 | 必需 | 说明 |
|------|------|------|
| `baseUrl` | 自定义 Provider 必需 | API 端点 URL |
| `api` | 自定义 Provider 必需 | `openai-completions` / `openai-responses` / `anthropic-messages` / `google-generative-ai` |
| `apiKey` | 否 | API Key；支持 `!cmd`、`$ENV`、字面值 |
| `headers` | 否 | 自定义请求头 |
| `authHeader` | 否 | 自动添加 `Authorization: Bearer` |
| `models` | 否 | 模型数组 |
| `modelOverrides` | 否 | 覆盖内置模型字段 |
| `compat` | 否 | Provider 级兼容性设置 |

### 模型级字段

| 字段 | 必需 | 默认 | 说明 |
|------|------|------|------|
| `id` | 是 | — | API 模型标识符 |
| `name` | 否 | `id` | 显示名，不替换页脚 id |
| `api` | 否 | Provider 的 | 覆盖 API 类型 |
| `reasoning` | 否 | `false` | 支持 extended thinking |
| `thinkingLevelMap` | 否 | — | Thinking level 映射，`null`=不支持 |
| `input` | 否 | `["text"]` | `["text"]` 或 `["text","image"]` |
| `contextWindow` | 否 | `128000` | 上下文窗口 Token |
| `maxTokens` | 否 | `16384` | 最大输出 Token |
| `cost` | 否 | 全零 | `{input, output, cacheRead, cacheWrite}` 每百万 Token |
| `compat` | 否 | Provider 的 | 模型级兼容性覆盖 |

## 常见操作指引

### 添加新 Provider + 模型

用户说 "帮我加一个 Ollama 的 qwen2.5-coder:7b" 时：
1. 确认 Provider 类型（本地/云端）→ 选择 API 类型（Ollama → `openai-completions`）
2. 确认 `baseUrl`（Ollama 默认 `http://localhost:11434/v1`）
3. 构建最小模型条目 `{"id":"qwen2.5-coder:7b"}`
4. 加入 `models` 数组
5. 如需 compat 细节，读 `references/compat-openai.md`

### 切换模型

用户说 "切换到 claude-sonnet-4" 时：模型切换不由 `models.json` 控制，告知用户使用 pi CLI：`pi --model "claude-sonnet-4"` 或在 `/model` 界面选择。

但如果用户意图是**添加某个模型以便后续切换**，按添加模型处理。

### 修改 compat

用户说 "Ollama 不支持 developer role" 时：
1. 读 `references/compat-openai.md` 确认字段
2. 在 Provider 级添加 `"compat": {"supportsDeveloperRole": false}`
3. 如需也禁用 reasoning effort，添加 `"supportsReasoningEffort": false`

### 添加 reasoning 模型 + thinkingLevelMap

用户说 "加一个 DeepSeek 模型，支持推理" 时：
1. 读 `references/thinking-level-map.md`
2. 设置 `"reasoning": true`
3. 根据用户对 thinking level 的描述构建 `thinkingLevelMap`

### 配置路由（OpenRouter / Vercel）

用户提到 "OpenRouter" 或 "路由" 时，读 `references/compat-openai.md` 中的 OpenRouter/Vercel 路由部分。

### 删除 Provider 或模型

- 删除整个 Provider：移除 `providers` 中对应 key
- 删除特定模型：从 Provider 的 `models` 数组中移除对应条目
- 删除 modelOverrides 中的条目：移除对应 key

### 查询现有配置

用户说 "列出我的模型" 时：读取 `models.json` 并格式化展示。

## 参考文档

需要字段级别的详细说明时，读取对应文件：

| 文件 | 内容 |
|------|------|
| `references/models-json.md` | models.json 文件结构、合并语义 |
| `references/provider-config.md` | Provider 字段详解、值解析、认证 |
| `references/model-config.md` | 模型字段完整说明 |
| `references/api-types.md` | 四种 API 类型及选择指南 |
| `references/compat-openai.md` | OpenAI 兼容性所有字段及 thinkingFormat |
| `references/compat-anthropic.md` | Anthropic 兼容性字段 |
| `references/thinking-level-map.md` | thinkingLevelMap 值语义和示例 |
| `references/examples.md` | 12 种典型配置完整范例 |

## 注意事项

- 编辑后无需重启 Pi，`/model` 打开时自动重载
- 对无密钥本地服务器（Ollama 等），`apiKey` 填占位值（如 `"ollama"`），通过 `/login` 或 `--api-key` 传任意值
- 模型同名 `id` 会覆盖已有条目（upsert）
- 内置 Provider 的 `modelOverrides` 只改字段不替换模型列表
- `name` 不影响页脚/状态栏显示，仅用于匹配和次要详情
- 所有 JSON 编辑必须保持合法 JSON 格式
