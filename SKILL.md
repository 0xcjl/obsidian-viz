---
name: obsidian-viz
description: |
  生成 Obsidian 兼容的可视化文件（Excalidraw / Mermaid / Canvas）。
  
  支持两种输入方式：
  1. 文字描述：从概念/流程描述创建图表
  2. 图片输入：理解 IM 图片（截图、流程图、架构图）并重绘为可编辑格式
  
  输出格式：
  - Obsidian 模式（默认）：.md / .canvas，可在 Obsidian 中直接打开
  - 标准模式：.mmd / .excalidraw / .html，通用格式
  
  触发场景：
  - 用户发送图片（自动触发图片理解模块）
  - 用户描述概念/流程（直接创建图表）
  - 明确要求"Obsidian 格式"或"标准格式"
  
  触发词：Excalidraw、画图、流程图、思维导图、Mermaid、Canvas、
  可视化、时序图、状态图、知识图谱、在Obsidian打开、生成图表文件、
  关系图、对比图、时间线、层级图、矩阵图、自由布局、手绘图、
  甘特图、ER图、交互图表、导出图表、obsidian图表、
  excalidraw、mermaid、canvas、mind map、flowchart、
  sequence diagram、visualize、animate、diagram
---

# Obsidian Viz Skill

生成 Obsidian 兼容的可视化文件，支持从文字描述或图片输入创建图表。

## 处理流程

### Step 0 - 输入类型判断

**如果用户发送图片**：
1. 加载 `modules/image-reader.md`
2. 执行图片类型识别与内容提取
3. 输出 Markdown 结构化摘要
4. 如果图片包含图表，继续到 Step 1
5. 如果图片仅为文字/截图，流程结束

**如果用户提供文字描述**：
- 直接进入 Step 1

### Step 1 - 工具选择

加载 `modules/chart-router.md`，根据内容类型选择最合适的工具：

- **Excalidraw**：手绘风格、架构图、自由布局、概念图
- **Mermaid**：技术文档、流程图、时序图、状态图、ER图
- **Canvas**：大型知识网络、交互式探索、数据可视化

### Step 2 - 格式规范加载

根据选择的工具，加载对应的 reference 文件：

- `mermaid` → `references/mermaid.md`
- `excalidraw` → `references/excalidraw.md`
- `canvas` → `references/canvas.md`

**重要**：生成任何内容之前，必须先读取对应的 reference 文件。

### Step 3 - 输出格式选择

**标准格式**（用户明确要求 "标准格式" 或 "excalidraw.com"）：
- Mermaid → `.mmd` 文件
- Excalidraw → `.excalidraw` 文件
- Canvas → `.html` 文件

**Obsidian 格式**（默认）：
- Mermaid → `.md` 文件（包含 mermaid 代码块）
- Excalidraw → `.md` 文件（包含 Excalidraw JSON）
- Canvas → `.canvas` 文件

### Step 4 - 生成文件

1. 严格遵循 reference 文件中的格式规范
2. 输出到 `~/.openclaw/workspace/outputs/<filename>.<ext>`
3. 向用户说明如何在 Obsidian 中打开文件

## 使用说明

### Excalidraw 文件

**Obsidian 模式** (`.md`)：
- 放入 vault 任意文件夹
- Obsidian 自动以画板模式打开
- 需安装 Excalidraw 插件

**标准模式** (`.excalidraw`)：
- 可在 excalidraw.com 直接打开
- 支持导入到任何 Excalidraw 实例

### Mermaid 文件

**Obsidian 模式** (`.md`)：
- 放入 vault 任意位置
- 普通预览模式即可渲染
- Obsidian 默认支持 Mermaid

**标准模式** (`.mmd`)：
- 可在支持 Mermaid 的编辑器中打开
- GitHub、GitLab 等平台原生支持

### Canvas 文件

**Obsidian 模式** (`.canvas`)：
- 放入 vault 任意位置
- 双击即可在 Canvas 视图中交互
- Obsidian 原生支持

**标准模���** (`.html`)：
- 在浏览器中打开
- 支持交互式探索

## 图表类型速查

| 需求 | 推荐工具 | 图表类型 |
|------|---------|---------|
| 工作流 / CI-CD | Excalidraw 或 Mermaid | flowchart |
| API 调用 / 消息交互 | Mermaid | sequenceDiagram |
| 组织架构 / 系统分层 | Excalidraw | hierarchy |
| 概念发散 / 头脑风暴 | Canvas 或 Excalidraw | mindmap |
| 状态机 / 生命周期 | Mermaid | stateDiagram-v2 |
| 项目时间线 | Excalidraw | timeline |
| A vs B 对比 | Excalidraw | comparison |
| 优先级矩阵 | Excalidraw | matrix |
| 大型知识网络 | Canvas | free-layout |
| 动画演示 | Excalidraw | animation mode |
| 数据库设计 | Mermaid | erDiagram |
| 类图 / 对象关系 | Mermaid | classDiagram |
| 项目排期 | Mermaid | gantt |

## 注意事项

1. **必须先加载 reference**：跳过此步骤会产生格式错误的文件
2. **中文支持**：所有工具均原生支持中文，无需转义
3. **文件路径**：输出文件统一放在 `~/.openclaw/workspace/outputs/` 目录
4. **降级策略**：如果优先工具失败，自动尝试备选工具
5. **节点数量限制**：超过 30 个节点时，建议用户拆分或使用 Canvas
