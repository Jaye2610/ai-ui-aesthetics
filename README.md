# ai-ui-aesthetics

> 前端美学增强插件，让 AI 生成漂亮、有层级、统一的前端 UI。

给 AI 编程代理用的**双形态插件**：**Skill（知识）+ MCP（工具）**，零框架依赖，可跨 Agent Harness（含 DeepSeek Harness、ZCode）使用。

## 它解决什么

AI 生成的前端常常"单调"——配色随手、间距无尺度、排版无层次、无质感。这个插件从四根支柱给 AI 装上"审美"：

| 支柱 | 位置 | 作用 |
| --- | --- | --- |
| 风格原型 | `skills/.../references/` | 7 种风格气质，写码前先选方向 |
| 审美知识 | `skills/.../knowledge/` | 设计原则 / 色彩 / 排版 / 留白 |
| 设计 Token | `skills/.../tokens/` | 可直接落地的色彩/字体/间距/阴影/动效规范 |
| 代码规则 | `skills/.../rules/` + `patterns/` | 组件 / 响应式 / 质感动效 / 检查清单 |

## Skill 部分

教 AI「**怎么想**」（脑子）：
- 单入口 `skills/ai-ui-aesthetics/SKILL.md`，工作流：选风格 → 定主色层级 → 用 Token 写码 → 美学自检。
- 详见该 SKILL 的「使用铁律」。

## MCP 部分

给 AI「**能做什么**」（手脚）：真实**抓参考站、扫图取色、产出配色建议**。

`mcp-server/`（Python + FastMCP）提供三个工具：

| 工具 | 作用 |
| --- | --- |
| `research_reference_site(url)` | 抓参考站，提取主色板/字体/圆角间距/亮暗 |
| `scan_colors(source)` | 从图片（URL/本地）量化主导色板 |
| `suggest_color_palette(style)` | 按风格原型给配色方案 + WCAG 对比度检查 |

运行与接入见 `mcp-server/README.md`。

## 作为 ZCode 插件安装

本目录符合 ZCode 插件规范：
- `skills/` → Skill（`SKILL.md`）
- `mcpServers` → 设计调研 MCP（见 `.zcode-plugin/plugin.json`）

把本目录作为本地插件源加入（Discover 标签 `+` → 本地目录），或解压到插件目录后启用。ZCode 会自动连接 MCP server（需 `mcp-server/.venv/` 已创建，见其 README）。

## 跨 Harness 接入

- DeepSeek Harness：字段映射与最小提示词片段见 `integration/deepseek-harness/INSTALL.md`。
- 通用 Harness：作为 Skill / 系统提示 / MCP 见 `integration/generic/INSTALL.md`。

## 目录结构

```
ai-ui-aesthetics/
├── .zcode-plugin/plugin.json    # ZCode 插件清单（skills + mcpServers）
├── skills/ai-ui-aesthetics/     # Skill 本体（SKILL.md + 四支柱）
├── mcp-server/                  # 设计调研 MCP（Python + FastMCP）
├── integration/                 # 跨 harness 接入说明
└── package.json                 # 包元数据（可选）
```

## 许可

MIT，见 `LICENSE`。
