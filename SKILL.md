---
name: obsidian-viz
description: Generate Obsidian-compatible visualization files (Excalidraw / Mermaid / Canvas). Supports text descriptions and image inputs, outputs editable diagrams in Obsidian or standard formats.
---

# Obsidian Viz Skill

Generate Obsidian-compatible visualization files from text descriptions or image inputs.

生成 Obsidian 兼容的可视化文件，支持从文字描述或图片输入创建图表。

## Processing Flow / 处理流程

### Step 0 - Input Type Detection / 输入类型判断

**If user sends an image / 如果用户发送图片**：
1. Load `modules/image-reader.md` / 加载图片阅读模块
2. Execute image type recognition and content extraction / 执行图片类型识别与内容提取
3. Output structured Markdown summary / 输出 Markdown 结构化摘要
4. If image contains diagrams, proceed to Step 1 / 如果图片包含图表，继续到 Step 1
5. If image is text/screenshot only, end process / 如果图片仅为文字/截图，流程结束

**If user provides text description / 如果用户提供文字描述**：
- Proceed directly to Step 1 / 直接进入 Step 1

### Step 1 - Tool Selection / 工具选择

Load `modules/chart-router.md` and select the most appropriate tool based on content type:

加载 `modules/chart-router.md`，根据内容类型选择最合适的工具：

- **Excalidraw**: Hand-drawn style, architecture diagrams, free layout, concept maps / 手绘风格、架构图、自由��局、概念图
- **Mermaid**: Technical documentation, flowcharts, sequence diagrams, state diagrams, ER diagrams / 技术文档、流程图、时序图、状态图、ER图
- **Canvas**: Large knowledge networks, interactive exploration, data visualization / 大型知识网络、交互式探索、数据可视化

### Step 2 - Format Specification Loading / 格式规范加载

Load the corresponding reference file based on the selected tool:

根据选择的工具，加载对应的 reference 文件：

- `mermaid` → `references/mermaid.md`
- `excalidraw` → `references/excalidraw.md`
- `canvas` → `references/canvas.md`

**Important / 重要**: Must read the corresponding reference file before generating any content.

必须先读取对应的 reference 文件，再生成任何内容。

### Step 3 - Output Format Selection / 输出格式选择

**Standard Format / 标准格式** (when user explicitly requests "standard format" or "excalidraw.com" / 用户明确要求 "标准格式" 或 "excalidraw.com")：
- Mermaid → `.mmd` file / `.mmd` 文件
- Excalidraw → `.excalidraw` file / `.excalidraw` 文件
- Canvas → `.html` file / `.html` 文件

**Obsidian Format / Obsidian 格式** (default / 默认)：
- Mermaid → `.md` file (with mermaid code block) / `.md` 文件（包含 mermaid 代码块）
- Excalidraw → `.md` file (with Excalidraw JSON) / `.md` 文件（包含 Excalidraw JSON）
- Canvas → `.canvas` file / `.canvas` 文件

### Step 4 - Generate File / 生成文件

1. Strictly follow format specifications in reference file / 严格遵循 reference 文件中的格式规范
2. Output to `~/.openclaw/workspace/outputs/<filename>.<ext>` / 输出到指定目录
3. Explain to user how to open the file in Obsidian / 向用户说明如何在 Obsidian 中打开文件

## Usage Instructions / 使用说明

### Excalidraw Files / Excalidraw 文件

**Obsidian Mode / Obsidian 模式** (`.md`)：
- Place in any vault folder / 放入 vault 任意文件夹
- Obsidian automatically opens in canvas mode / Obsidian 自动以画板模式打开
- Requires Excalidraw plugin / 需安装 Excalidraw 插件

**Standard Mode / 标准模式** (`.excalidraw`)：
- Can be opened directly at excalidraw.com / 可在 excalidraw.com 直接打开
- Supports import to any Excalidraw instance / 支持导入到任何 Excalidraw 实例

### Mermaid Files / Mermaid 文件

**Obsidian Mode / Obsidian 模式** (`.md`)：
- Place anywhere in vault / 放入 vault 任意位置
- Renders in normal preview mode / 普通预览模式即可渲染
- Obsidian supports Mermaid by default / Obsidian 默认支持 Mermaid

**Standard Mode / 标准模式** (`.mmd`)：
- Can be opened in Mermaid-compatible editors / 可在支持 Mermaid 的编辑器中打开
- Natively supported by GitHub, GitLab, etc. / GitHub、GitLab 等平台原生支持

### Canvas Files / Canvas 文件

**Obsidian Mode / Obsidian 模式** (`.canvas`)：
- Place anywhere in vault / 放入 vault 任意位置
- Double-click to interact in Canvas view / 双击即可在 Canvas 视图中交互
- Natively supported by Obsidian / Obsidian 原生支持

**Standard Mode / 标准模式** (`.html`)：
- Open in browser / 在浏览器中打开
- Supports interactive exploration / 支持交互式探索

## Chart Type Quick Reference / 图表类型速查

| Need / 需求 | Recommended Tool / 推荐工具 | Chart Type / 图表类型 |
|------|---------|---------|
| Workflow / CI-CD / 工作流 | Excalidraw or Mermaid | flowchart |
| API calls / Message interaction / API 调用 / 消息交互 | Mermaid | sequenceDiagram |
| Organization / System hierarchy / 组织架构 / 系统分层 | Excalidraw | hierarchy |
| Concept divergence / Brainstorming / 概念发散 / 头脑风暴 | Canvas or Excalidraw | mindmap |
| State machine / Lifecycle / 状态机 / 生命周期 | Mermaid | stateDiagram-v2 |
| Project timeline / 项目时间线 | Excalidraw | timeline |
| A vs B comparison / 对比 | Excalidraw | comparison |
| Priority matrix / 优先级矩阵 | Excalidraw | matrix |
| Large knowledge network / 大型知识网络 | Canvas | free-layout |
| Animation demo / 动画演示 | Excalidraw | animation mode |
| Database design / 数据库设计 | Mermaid | erDiagram |
| Class diagram / Object relationships / 类图 / 对象关系 | Mermaid | classDiagram |
| Project schedule / 项目排期 | Mermaid | gantt |

## Notes / 注意事项

1. **Must load reference first / 必须先加载 reference**: Skipping this step will produce incorrectly formatted files / 跳过此步骤会产生格式错误的文件
2. **Chinese support / 中文支持**: All tools natively support Chinese, no escaping needed / 所有工具均原生支持中文，无需转义
3. **File path / 文件路径**: Output files are uniformly placed in `~/.openclaw/workspace/outputs/` directory / 输出文件统一放在指定目录
4. **Fallback strategy / 降级策略**: If primary tool fails, automatically try alternative tool / 如果优先工具失败，自动尝试备选工具
5. **Node count limit / 节点数量限制**: For more than 30 nodes, recommend user to split or use Canvas / 超过 30 个节点时，建议用户拆分或使用 Canvas
