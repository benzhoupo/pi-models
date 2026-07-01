# models.json 文件结构总览

`models.json` 默认位于 `~/.pi/agent/models.json`（Windows: `%USERPROFILE%\.pi\agent\models.json`）。文件在每次打开 `/model` 时重新加载，编辑后无需重启 Pi。

## 顶层结构

```json
{
  "providers": {
    "<provider-id>": { ... },
    ...
  }
}
```

只有 `providers` 一个顶层字段，值为 `Provider ID → Provider配置` 的映射。

## Provider ID 规则

- **内置 Provider**：使用内置名称（如 `anthropic`, `openai`, `google`, `openrouter`）可覆盖/扩展内置 Provider，不定义 `models` 时原模型保留
- **自定义 Provider**：任意唯一 ID（如 `ollama`, `my-google`, `deepseek-proxy`），必须提供 `baseUrl` 和 `api`

## 合并语义

内置 Provider 的合并规则：
- 不定义 `models` → 内置模型全部保留，只覆盖 `baseUrl`/`apiKey`/`headers` 等 Provider 级别字段
- 定义 `models` → 内置模型保留，自定义模型按 `id` upsert（同 id 替换，新 id 添加）
- `modelOverrides` → 覆盖特定内置模型的部分字段，不替换完整模型列表

## 最小配置模板

```json
{
  "providers": {}
}
```

从空配置开始即可逐步添加 Provider 和模型。
