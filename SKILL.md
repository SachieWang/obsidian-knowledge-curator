---
name: obsidian-knowledge-curator
description: >-
  Standardizes and systematically organizes Obsidian knowledge bases.
  Enforces strict workflows: vault health check, YAML formatting, taxonomy
  classification, and MOC (Map of Content) updates. Relies entirely on
  Obsidian MCP tools to prevent broken backlinks.
license: Apache-2.0
compatibility: Requires obsidian MCP server
metadata:
  version: "1.0"
---

# Obsidian Knowledge Curator

## Overview
This skill transforms the agent into an Obsidian "Knowledge Curator" (知识策展人). It guides the agent to systematically process scattered markdown notes into highly organized, metadata-rich, and interconnected knowledge graphs. It prevents "knowledge islands" and guarantees backlink safety by enforcing the use of the `obsidian` MCP server over raw filesystem modifications.

## Dependencies
- **`obsidian` MCP Server**: Mandatory. The skill entirely relies on the Obsidian MCP for safely manipulating the vault (`vault_read`, `vault_write`, `vault_move`, `vault_get_document_map`, `search_simple`). 

## Quick Start
User request example:
> "Please use the `obsidian-knowledge-curator` skill to organize my `20_Tech_Atlas` folder. I have some new scattered notes there, please analyze them and integrate them into my existing MOC."

## Workflow

### 1. Health Check & Auditing (现状审计与分析)
- **Action**: Use `vault_get_document_map` or `search_simple` to locate and list the target scattered files.
- **Action**: Briefly `vault_read` the files to evaluate their content domain and check if they have valid YAML frontmatter.
- **Output**: Present a **Health Check Report (体检表)** summarizing the current state of the folder and the missing metadata.

### 2. Planning & Classification (制定计划)
- **Action**: Based on the audited content, autonomously deduce a reasonable taxonomy/folder classification strategy (unless the user provided one).
- **Output**: Present an **Implementation Plan (实施计划书)** proposing the new directory structure, the expected tags, and how the MOC will be integrated. Wait for user approval if the changes are massive.

### 3. Frontmatter Standardization (元数据注入)
- **Action**: For each note lacking proper metadata, use `vault_read` to extract the content.
- **Action**: Prepend or repair the standard YAML frontmatter.
  ```yaml
  ---
  title: <filename>
  tags:
    - <tag1>
    - <tag2>
  date: YYYY-MM-DD
  ---
  ```
- **Action**: Use `vault_write` to overwrite the note with the standardized metadata.

### 4. Safe Migration (安全归档)
- **Action**: Move the processed notes to their respective classified subdirectories.
- **CRITICAL**: You MUST use the `vault_move` MCP tool to move files. **NEVER** use bash commands like `mv` or standard file writing tools to change paths. `vault_move` inherently triggers Obsidian's internal backlink updating engine, ensuring no wiki-links (`[[...]]`) are broken during the reorganization.

### 5. MOC Integration (织入索引)
- **Action**: Find the relevant MOC (Map of Content) file. If none exists, create one.
- **Action**: Use `vault_read` to analyze the MOC structure. 
- **Action**: Weave the newly organized notes into the MOC using standard short wiki-links (e.g., `[[Note Title]]`) categorized cleanly by domain. 
- **Action**: Use `vault_write` to save the updated MOC.

### 6. Final Reporting (收尾汇报)
- **Output**: Create a **Task Breakdown Table (拆解任务步骤表)** to check off tasks, and finally deliver a **Completion Report (完成报告)** detailing the exact folders created, files moved, and MOC sections updated.

## Error Handling Strategy
- **Safe Autonomous Retry**: If an MCP tool fails (e.g., "File not found" or "Timeout"), the agent should attempt a safe autonomous action. For example, use `search_simple` to find the correct file path or try verifying the MCP server connection.
- **Transparency**: Log these retry attempts in the conversation so the user knows what the agent is doing.
- **Graceful Handoff**: If the autonomous fix fails after 1-2 attempts, gracefully pause execution, present the exact error to the user, and ask for manual assistance (e.g., "Please check if your Obsidian MCP server is running").

## Common Mistakes
1. **Using bash `mv` or raw Python file moves**: This is strictly forbidden. It bypasses Obsidian's link resolution and destroys backlinks. Always use `vault_move`.
2. **Forgetting the YAML Frontmatter**: Do not just move files; a Curator must ensure every piece of knowledge is fully tagged.
3. **Leaving Knowledge Islands**: Organizing files into folders is not enough. Failing to link them into an MOC violates the core principle of a connected knowledge base.
