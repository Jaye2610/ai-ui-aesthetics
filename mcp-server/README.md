# Design Research MCP Server

`ai-ui-aesthetics` 插件的**工具型**组件（MCP）：让 AI 能**真实抓参考站、扫图取色、产出配色建议**，用来生成更有依据、更漂亮的前端 UI。

这是本插件的「手脚」部分——与 `SKILL.md`（「脑子」，教审美）互补。

## 工具一览

| 工具 | 入参 | 返回 |
| --- | --- | --- |
| `research_reference_site(url)` | 参考站完整 URL | 标题、主色板（高频色+占比）、字体族、圆角/间距线索、亮暗主题 |
| `scan_colors(source, max_colors=8)` | 图片 URL 或本地路径 | 主导色板（hex + 占比） |
| `suggest_color_palette(style)` | 风格关键词 | 配色方案（主/强调/中性/语义）+ WCAG 对比度检查 |

`style` 支持：`minimal/极简`、`glass/玻璃`、`editorial/编辑`、`bold/撞色`、`dark/深色`、`soft/柔`、`data/数据`。

## 运行

本项目自带 uv 虚拟环境（`.venv/`），零手动配置：

```bash
cd mcp-server
uv run --with <deps> python mcp_server.py      # 或直接
./.venv/Scripts/python.exe mcp_server.py       # Windows
```

> 首次使用若 `.venv/` 不存在：`uv venv && uv pip install -r requirements.txt`。

## 接入 ZCode / 其他 harness

### 方式 A：随本插件自动连接
插件 `.zcode-plugin/plugin.json` 已注册 `mcpServers`，ZCode 安装插件后自动连接（见该文件 `mcpServers.command`）。ZCode 会**自动信任并连接插件提供的 MCP server**。

### 方式 B：手动注册（通用 harness）
在任何支持的 MCP 客户端添加 stdio server，命令为：

```json
{
  "command": "<此目录绝对路径>/.venv/Scripts/python.exe",
  "args": ["<此目录绝对路径>/mcp_server.py"]
}
```

Linux/macOS 用 `.venv/bin/python`。

## 边界与伦理
- 仅做**设计调研**：抓取限速、超时、遵 robots；提炼设计原则与 token，**不复制目标站点的像素资产**。
- 不持久化任何用户数据，不追踪。
- 请确保你有权访问所抓取的站点。

## 依赖
见 `requirements.txt`：`fastmcp`、`httpx`、`beautifulsoup4`、`Pillow`。

## 冒烟测试
```bash
./.venv/Scripts/python.exe -c "import mcp_server as m; from asyncio import run; print([t.name for t in run(m.mcp.list_tools())])"
```
应输出三个工具名。更完整的 MCP 握手验证见 `tests/`（可选）。
