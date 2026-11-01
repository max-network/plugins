# DRY.codes

Reuse before you write. The plugin helps your agent find existing code and docs to reuse
before it writes something new, and catch near-duplicates before it finishes.

## What's inside

| Component | Purpose |
| --- | --- |
| `dry-codes` MCP server (`https://dry.codes/mcp`) | Searches indexed code and docs (READMEs, ADRs, conventions) by text and by meaning, and reports duplication. |
| `dry-codes` skill | Makes the agent search, read the strongest hits and check for duplicates before it writes a new function, component, type or endpoint. |

## Sign-in

You sign in with GitHub the first time the agent calls the server. You then pick which corpus
to search: one of your own indexed DRY.codes endpoints, or the public corpus of repos shared on
DRY.codes. Manage endpoints at https://dry.codes.

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install dry-codes@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add dry-codes@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./dry-codes`.

Any other MCP client:

```json
{
  "mcpServers": {
    "dry-codes": { "url": "https://dry.codes/mcp" }
  }
}
```

## Try it

- "Add a date-formatting helper." The agent searches for an existing one first.
- "Is there already a retry wrapper for fetch somewhere in our repos?"
- "Which files in this repo duplicate each other?"

## Layout

```
dry-codes/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
├── assets/                      icons
└── skills/dry-codes/SKILL.md
```

More at https://dry.codes.
