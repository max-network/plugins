# Max Network plugins

A plugin marketplace for [Claude Code](https://claude.com/claude-code) and Codex.

## `legal` — patient-rights advocate

Turns your AI into a **patient-rights advocate**. It bundles:

- **The `patient-rights-advocate` skill** — when you describe a healthcare problem (a denied
  treatment or claim, refused medical records, a consent/privacy issue, a billing dispute,
  poor care, discrimination, cross-border care), the agent finds the law on your side and
  helps you act on it.
- **The hosted MCP server** — `https://legal.maxhealth.tech/mcp`, covering primary law and
  case law across the **US, EU, Austria, Germany, and the Netherlands**. On first use you
  sign in (Google or Max Health). US federal + EU law are included free; US state and EU
  member-state laws are add-ons.

### Install — Claude Code

```
/plugin marketplace add max-network/plugins
/plugin install legal@max-network
```

Then restart Claude Code (or `/reload-plugins`). Try it locally first with
`claude --plugin-dir ./legal`.

### Install — Codex

```
codex plugin marketplace add max-network/plugins
codex plugin add legal@max-network
```

`marketplace add` registers the source; `plugin add` installs the plugin
(`legal@max-network` = the `legal` plugin from the `max-network` marketplace). Restart
Codex after installing.

### Manual MCP config (any MCP client)

If your client isn't plugin-aware, add the server directly:

```json
{
  "mcpServers": {
    "legal": { "url": "https://legal.maxhealth.tech/mcp" }
  }
}
```

You'll be asked to sign in the first time.

## Layout

```
.claude-plugin/marketplace.json     # Claude Code marketplace
.agents/plugins/marketplace.json    # Codex marketplace
legal/                              # the plugin
├── .claude-plugin/plugin.json      # Claude Code manifest
├── .codex-plugin/plugin.json       # Codex manifest
├── .mcp.json                       # Claude: { mcpServers: { legal: { type: http, url } } }
├── .mcp.codex.json                 # Codex MCP server config
└── skills/patient-rights-advocate/SKILL.md
```

Learn more: https://legal.maxhealth.tech
