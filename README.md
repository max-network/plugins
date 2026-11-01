# Max Network plugins

A plugin marketplace for [Claude Code](https://claude.com/claude-code) and Codex. Each plugin
lives in its own folder, bundles a hosted MCP server with one or more skills, and has its own
README.

| Plugin | Folder | What it does |
| --- | --- | --- |
| `legal-research` | [`legal/`](legal/README.md) | Answers legal questions from primary law and case law, with citations, and helps patients act on healthcare disputes. |
| `dry-codes` | [`dry-codes/`](dry-codes/README.md) | Finds existing code and docs to reuse before the agent writes new code, and catches near-duplicates. |
| `drypdf` | [`drypdf/`](drypdf/README.md) | Searches, reads, uploads and organizes the PDFs in your DRYPDF account. |
| `chess` | [`chess/`](chess/README.md) | Plays chess on an interactive board against Stockfish, the model or people in a live lobby, then explains the game. |
| `scribe` | [`scribe/`](scribe/README.md) | Turns what a patient describes, or a clinical document, into structured records someone checks before anything is saved. |

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

Replace `<plugin>` with `legal-research`, `dry-codes`, `drypdf`, `chess` or `scribe`, then restart the client.

## Layout

```
.claude-plugin/marketplace.json     Claude Code marketplace
.agents/plugins/marketplace.json    Codex marketplace
legal/                              one plugin per folder
dry-codes/
drypdf/
chess/
scribe/
```

Every plugin folder holds `.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`, `.mcp.json`,
`.mcp.codex.json` and `skills/`. Each folder is a self-contained plugin, so point a directory
submission or `claude --plugin-dir` at the plugin folder, never the repo root.

Both marketplace files are generated from the Max Health product catalog, which decides which
plugins are listed and how they are described. Edit the catalog and regenerate rather than
changing them by hand; a scheduled check flags any drift.
