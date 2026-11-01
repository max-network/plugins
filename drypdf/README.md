# DRYPDF

Turns your AI into a librarian for your PDFs. The plugin lets your agent search, read,
upload and organize the PDFs stored in your DRYPDF account, and answer from their actual text.

## What's inside

| Component | Purpose |
| --- | --- |
| `drypdf` MCP server (`https://drypdf.com/mcp`) | Keyword and semantic search, full-text reading, upload, download, groups and duplicate detection over your stored PDFs. |
| `drypdf-librarian` skill | Tells the agent to find the right document, read what it says before answering, and keep the collection organized. |

## Sign-in

You sign in the first time the agent calls the server, and pick the account (personal or a
team) the connection uses. Every tool is scoped to that account.

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install drypdf@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add drypdf@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./drypdf`.

Any other MCP client:

```json
{
  "mcpServers": {
    "drypdf": { "url": "https://drypdf.com/mcp" }
  }
}
```

## Try it

- "Find the contract that mentions the penalty clause."
- "Upload this PDF and put it in a 'Taxes 2025' group."
- "Which of my documents are duplicates?"

## Layout

```
drypdf/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
└── skills/drypdf-librarian/SKILL.md
```

More at https://drypdf.com.
