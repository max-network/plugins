# Max Network plugins

A plugin marketplace for [Claude Code](https://claude.com/claude-code) and Codex. Each plugin
lives in its own folder, bundles a hosted MCP server with one or more skills, and has its own
README.

| Plugin | Folder | What it does |
| --- | --- | --- |
| `legal` | [`legal/`](legal/README.md) | Answers legal questions from primary law and case law, with citations, and helps patients act on healthcare disputes. |
| `dry-codes` | [`dry-codes/`](dry-codes/README.md) | Finds existing code and docs to reuse before the agent writes new code, and catches near-duplicates. |
| `drypdf` | [`drypdf/`](drypdf/README.md) | Searches, reads, uploads and organizes the PDFs in your DRYPDF account. |

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install <plugin>@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add <plugin>@max-network
```

Replace `<plugin>` with `legal`, `dry-codes` or `drypdf`, then restart the client.

## Layout

```
.claude-plugin/marketplace.json     Claude Code marketplace
.agents/plugins/marketplace.json    Codex marketplace
legal/                              one plugin per folder
dry-codes/
drypdf/
```

Every plugin folder holds `.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`, `.mcp.json`,
`.mcp.codex.json` and `skills/`. Each folder is a self-contained plugin, so point a directory
submission or `claude --plugin-dir` at the plugin folder, never the repo root.
