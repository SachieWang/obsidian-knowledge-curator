# Obsidian Knowledge Curator

[Read in Chinese (中文版)](./README_zh.md)

This Agent Skill transforms your AI assistant into an Obsidian "Knowledge Curator." It systematically processes scattered markdown notes into highly organized, metadata-rich, and interconnected knowledge graphs.

## Core Features

- **Health Check & Auditing**: Locates scattered notes and evaluates their missing metadata and domains.
- **Taxonomy & Classification**: Autonomously deduces and applies a reasonable folder taxonomy.
- **Frontmatter Standardization**: Injects standard YAML metadata (title, tags, date) into notes.
- **Safe Vault Migration**: Safely moves files into organized structures using the `obsidian` MCP Server without breaking wiki-links.
- **MOC Integration**: Automatically weaves new knowledge into Maps of Content (MOC).

## Dependencies

- **Obsidian MCP Server**: This skill relies heavily on the official Obsidian MCP server tools (`vault_read`, `vault_write`, `vault_move`, `search_simple`, etc.).
- Ensure your Obsidian MCP server is properly configured and running before using this skill.

## Usage

Simply ask the agent to act as your knowledge curator. For example:

> "Please use the `obsidian-knowledge-curator` skill to organize my `20_Tech_Atlas` folder. I have some new scattered notes there, please analyze them and integrate them into my existing MOC."

## Safety First

This skill strictly avoids using raw OS commands (like `mv` or `cp`) or direct file I/O to reorganize your vault. It routes all modifications through Obsidian's native APIs via the MCP server, ensuring that all `[[wiki-links]]` are safely updated and resolved by Obsidian's internal engine.

## License

This project is licensed under the [Apache License 2.0](./LICENSE).
