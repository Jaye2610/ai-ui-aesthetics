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

**导入即用**：插件清单用 `${ZCODE_PLUGIN_ROOT}` 模板变量指到插件自带的启动器 `mcp-server/launch_mcp.py`，ZCode 安装时会自动把它解析为插件在本机的实际根目录。启动器会**自动创建 `.venv` 并安装依赖**（仅首次运行；之后直接复用），因此克隆 / 导入后**无需手动配置 MCP 路径、也无需自己建虚拟环境**。只要机器上有 `python` 3.10+ 即可。

ZCode 的 **Settings → Plugin Management**（插件管理）分 **Installed**（已安装）和 **Discover**（发现）两个标签。下面的几种方式任选其一，目的都是让 ZCode 识别到本插件的 `.zcode-plugin/plugin.json`。安装后到 **Installed** 标签确认插件处于**启用**状态。

### 方式一：从开源仓库克隆安装（GitHub）

从 GitHub 仓库克隆到本地，然后作为本地插件目录加入 ZCode：

```bash
# 克隆仓库到任意目录（以 D:\repos 为例）
cd D:\repos
git clone git@github.com:Jaye2610/ai-ui-aesthetics.git
cd ai-ui-aesthetics
```

然后在 ZCode **Settings → Plugin Management → Discover** 点 **`+` → 选择本地目录**，选中 `D:\repos\ai-ui-aesthetics`（含 `.zcode-plugin/plugin.json` 的根目录），启用即可。首次使用 MCP 工具时 ZCode 会自动拉起启动器、完成环境自举，无需其它手动步骤。

（可选）想提前验证环境，可手动跑一次启动器自带的冒烟测试，正常会输出三个工具名：

```bash
python mcp-server/launch_mcp.py --smoke
```

### 方式二：本地目录作为插件源（推荐，开发调试用）

如果你已有一份可用的本地副本（如刚克隆的、或正在开发的本目录），直接在 Discover 标签 **`+` → 选择本地目录**选中它，启用即可。与方式一在安装步骤上相同，区别只是无需先克隆。

### 方式三：从市场安装

若你的环境已配置了包含本插件的市场，在 Discover 标签 **`+`** 选择市场来源并启用。市场推送尚未配置时，本方式暂不可用，请用方式一。

> 只有首次使用 MCP（抓站 / 扫图 / 配色建议）时才需要联网安装依赖；仅启用 Skill 不依赖 venv，也不占用启动时间。

## 跨 Harness 接入

- DeepSeek Harness：字段映射与最小提示词片段见 `integration/deepseek-harness/INSTALL.md`。
- 通用 Harness：作为 Skill / 系统提示 / MCP 见 `integration/generic/INSTALL.md`。

## 目录结构

```
ai-ui-aesthetics/
├── .zcode-plugin/plugin.json    # ZCode 插件清单（skills + mcpServers，模板变量自举）
├── skills/ai-ui-aesthetics/     # Skill 本体（SKILL.md + 四支柱）
├── mcp-server/                  # 设计调研 MCP（Python + FastMCP）
│   └── launch_mcp.py            # 自举启动器：自动建 .venv + 装依赖
├── integration/                 # 跨 harness 接入说明
└── package.json                 # 包元数据（可选）
```

## 许可

MIT，见 `LICENSE`。
