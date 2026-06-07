# Obsidian Knowledge Curator (Obsidian 知识策展人)

[Read in English](./README.md)

本 Agent 技能（Skill）将你的 AI 助手转化为一位 Obsidian “知识策展人”。它能够系统地将零散的 Markdown 笔记转化为高度结构化、元数据丰富且相互关联的知识图谱。

## 核心特性

- **现状审计与分析**：自动定位零散笔记，并评估其缺失的元数据和所属领域。
- **分类规划**：根据笔记内容自主推断并应用合理的文件夹分类体系。
- **元数据标准化**：为缺乏元数据的笔记自动注入标准 YAML frontmatter（标题、标签、日期等）。
- **安全归档**：使用 `obsidian` MCP 服务器的官方工具安全移动文件，绝对不会破坏现有的双向链接。
- **MOC 织入**：将新整理的知识自动编织到对应的 MOC（Map of Content，内容映射表）中。

## 依赖要求

- **Obsidian MCP Server**：本技能深度依赖 Obsidian MCP 服务器提供的官方工具（如 `vault_read`, `vault_write`, `vault_move`, `search_simple` 等）。
- 使用本技能前，请确保你的 Obsidian MCP 服务器已正确配置并启动。

## 使用方法

直接向 Agent 发送请求即可。例如：

> "请使用 `obsidian-knowledge-curator` 技能帮我整理一下 `20_Tech_Atlas` 文件夹。里面有一些新的零散笔记，请分析它们并整合到我现有的 MOC 中。"

## 安全第一

本技能严格禁止使用底层系统命令（如 `mv` 或 `cp`）或直接的文件读写来重组你的金库（Vault）。所有的修改都会通过 MCP 服务器路由至 Obsidian 原生 API，从而确保所有 `[[双向链接]]` 都能被 Obsidian 内部引擎安全地更新和解析。

## 开源协议

本项目采用 [Apache License 2.0](./LICENSE) 协议开源。
