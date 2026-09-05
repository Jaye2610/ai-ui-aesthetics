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

ZCode 的 **Settings → Plugin Management**（插件管理）分 **Installed**（已安装）和 **Discover**（发现）两个标签。下面的几种方式任选其一，目的都是让 ZCode 识别到本插件的 `.zcode-plugin/plugin.json`。安装后到 **Installed** 标签确认插件处于**启用**状态。

### 方式一：从开源仓库克隆安装（GitHub）

从 GitHub 仓库克隆到本地，然后作为本地插件目录加入 ZCode：

```bash
# 1. 克隆仓库到任意目录（以 D:\repos 为例）
cd D:\repos
git clone git@github.com:Jaye2610/ai-ui-aesthetics.git
cd ai-ui-aesthetics
```

```bash
# 2. 给 MCP server 创建 Python 虚拟环境并安装依赖（本仓库不含 .venv，必需）
cd mcp-server
uv venv
uv pip install -r requirements.txt
```

> 需要先安装 [uv](https://docs.astral.sh/uv/)；也可以改用系统 Python：`python -m venv .venv && .venv/Scripts/python.exe -m pip install -r requirements.txt`（Windows，Linux/macOS 为 `.venv/bin/python`）。

```bash
# 3. 回到仓库根目录，冒烟测试 MCP server 是否能启动（应输出三个工具名）
cd ..
mcp-server/.venv/Scripts/python.exe -c "import mcp_server as m; from asyncio import run; print([t.name for t in run(m.mcp.list_tools())])"
```

然后在 ZCode **Settings → Plugin Management → Discover** 点 **`+` → 选择本地目录**，选中 `D:\repos\ai-ui-aesthetics`（含 `.zcode-plugin/plugin.json` 的根目录），启用即可。

> ⚠️ `plugin.json` 里 `mcpServers.command/args` 目前是**写死的绝对路径**（指向开发机上的另一份副本）。克隆到你机器后，若 MCP 连不上，请打开 `plugin.json` 把这两处改成 `mcp-server/.venv/Scripts/python.exe` 和你 `mcp-server/mcp_server.py` 的**本机绝对路径**（Linux/macOS 用 `.venv/bin/python`），保存后重启插件连接。

### 方式二：本地目录作为插件源（推荐，开发调试用）

如果你已有一份可用的本地副本（如刚克隆的、或正在开发的本目录），直接在 Discover 标签 **`+` → 选择本地目录**选中它，启用即可。与方式一在安装步骤上相同，区别只是无需先克隆。

### 方式三：从市场安装

若你的环境已配置了包含本插件的市场，在 Discover 标签 **`+`** 选择市场来源并启用。市场推送尚未配置时，本方式暂不可用，请用方式一。

> 无论哪种方式，仅启用 Skill 不依赖 venv；只有使用 MCP（抓站 / 扫图 / 配色建议）时才需要 `.venv` 存在且路径正确。

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
