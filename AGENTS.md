# AGENTS.md — Pi Models Skill 仓库维护指引

## 仓库性质

这是一个 **AI Skill 仓库**，包含一个 skill 定义文件（`SKILL.md`）和若干参考文档（`references/`）。不是代码项目，没有构建/测试/CI。

## 文件职责

| 文件 | 职责 | 维护策略 |
|------|------|----------|
| `SKILL.md` | Skill 入口，含 YAML frontmatter + 工作流指令 + 字段速查表 | 精简，只放工作流和速查，具体字段细节放 references |
| `references/models-json.md` | models.json 顶层结构、Provider ID 规则、合并语义 | 结构说明 |
| `references/provider-config.md` | Provider 级字段完整说明、apiKey 值解析、认证优先级 | 字段详解 |
| `references/model-config.md` | 模型级字段完整说明、默认值 | 字段详解 |
| `references/api-types.md` | 四种 API 类型对比、选择指南 | 类型说明 |
| `references/compat-openai.md` | OpenAI compat 全部字段、thinkingFormat 枚举、路由配置 | 字段详解 |
| `references/compat-anthropic.md` | Anthropic compat 全部字段 | 字段详解 |
| `references/thinking-level-map.md` | thinkingLevelMap 值语义、示例 | 语义说明 |
| `references/examples.md` | 完整配置范例（12 个） | 可运行示例 |

## 修改策略

### 改什么文件

- **新增/修改工作流步骤** → 改 `SKILL.md`
- **新增/修改字段定义、字段说明** → 改 `references/` 中对应文件
- **新增配置范例** → 改 `references/examples.md`
- **SKILL.md 中的速查表与 references 冲突** → 以 references 为准，同步更新 SKILL.md
- **新增 API 类型** → 改 `references/api-types.md`，同步更新 `SKILL.md` 中的示例

### 不要改什么

- `SKILL.md` 的 YAML frontmatter 中的 `name: pi-models` 不改（skill 标识）
- `description` 字段仅在需要更新触发条件时修改
- `references/` 下文件的整体结构保持不变，只在末尾追加或在对应位置修改

### 编码约定

- 全部使用中文撰写
- Markdown 表格优先，字段速查用表格，语义说明用正文
- JSON 示例必须合法（无注释、无尾逗号）
- Provider ID / 字段名 / 值使用反引号包裹
- 文件名使用 kebab-case

## SKILL.md 结构

```
---
name: pi-models
description: <触发条件描述>
---

# Pi Models

## 配置文件路径
## 工作流
## 字段速查
## 常见操作指引
## 参考文档
## 注意事项
```

SKILL.md 是 skill 被加载时注入给 AI 的指令，决定了 AI 的行为方式。修改时注意：
- 工作流步骤必须明确、可执行
- 决策树（何时读哪个 reference）放在工作流或常见操作指引中
- 不要重复 references 中的完整内容，用表格速查 + 引用即可

## 参考文档格式约定

每个 `references/` 文件遵循统一结构：
- 标题说明该文件内容范围
- 表格速查（字段名、必需/默认值、说明）
- 关键概念详解（值语义、合并逻辑、特殊规则）
- 代码示例

## 与 Pi 产品的关系

- `models.json` 的实际路径：`~/.pi/agent/models.json`（Linux/macOS）或 `%USERPROFILE%\.pi\agent\models.json`（Windows）
- 编辑后 `/model` 打开时自动重载，无需重启
- 认证优先级：`/login`/`auth.json` > `--api-key` > Provider `apiKey`
- 这些产品行为仅在 Pi 更新时可能变化，本仓库应保持同步
