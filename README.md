# Pi Models

管理 [Pi 编码助手]((https://github.com/earendil-works/pi)) 的自定义模型配置（`models.json`）的 OpenCode Skill。

## 安装

将本仓库克隆到 OpenCode 的 skills 目录：

```
git clone <repo-url> ~/.config/opencode/skills/pi-models
```

或直接复制整个目录到 `~/.config/opencode/skills/pi-models/`。

## 功能

- **添加 Provider**：Ollama、OpenRouter、DeepSeek、Google AI Studio、vLLM / SGLang、Anthropic 代理 等
- **管理模型**：增删改查自定义模型条目
- **兼容性设置**：处理 developer role、reasoning effort、thinking format 等兼容问题
- **Thinking 配置**：设置 `reasoning` + `thinkingLevelMap`，控制推理行为
- **路由策略**：OpenRouter / Vercel AI Gateway 的模型路由配置
- **覆盖内置配置**：修改内置 Provider 的 baseUrl / apiKey，或覆盖特定模型字段

## 使用示例

在 OpenCode 中使用自然语言操作：

| 你说 | 效果 |
|------|------|
| "帮我加一个 Ollama 的 qwen2.5-coder" | 添加 Ollama Provider + 模型 |
| "Ollama 不支持 developer role，帮我修一下" | 添加 compat 兼容性配置 |
| "加一个 DeepSeek 模型，支持推理" | 添加 reasoning 模型 + thinkingLevelMap |
| "列出我的自定义模型" | 读取并展示现有配置 |
| "用 OpenRouter 路由，只走 bedrock" | 配置 OpenRouter 模型路由 |
| "把 Google 的 apiKey 改成环境变量" | 修改 Provider 的 apiKey |
| "删除 ollama 这个 provider" | 移除整个 Provider 配置 |

## 配置文件

Skill 编辑的文件路径：

- **Linux / macOS**：`~/.pi/agent/models.json`
- **Windows**：`%USERPROFILE%\.pi\agent\models.json`

编辑后无需重启 Pi，打开 `/model` 界面即可自动加载。

## 目录结构

```
pi-models/
├── SKILL.md               # Skill 定义（入口）
├── AGENTS.md              # 仓库维护指引
├── README.md              # 本文件
└── references/            # 参考文档（按需查阅）
    ├── models-json.md     # models.json 结构说明
    ├── provider-config.md # Provider 字段详解
    ├── model-config.md    # 模型字段详解
    ├── api-types.md       # API 类型选择指南
    ├── compat-openai.md   # OpenAI 兼容性字段
    ├── compat-anthropic.md# Anthropic 兼容性字段
    ├── thinking-level-map.md # thinking 级别映射
    └── examples.md        # 12 种完整配置范例
```

## 许可

Apache 2.0
