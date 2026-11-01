# Legal Research by Max Health

Answers legal questions from primary law, with citations. The plugin bundles the hosted
Legal MCP server and two skills that tell your agent how to use it.

## What's inside

| Component | Purpose |
| --- | --- |
| `legal` MCP server (`https://legal.maxhealth.tech/mcp`) | Searches statutes, regulations and case law and returns the actual provision text and decisions. |
| `legal-research` skill | Grounds any legal question in primary sources: what a rule requires, which deadline applies, how a court ruled, how two jurisdictions compare. |
| `patient-rights-advocate` skill | Helps a patient act on a denied treatment, a rejected coverage claim, a refused records request, a consent or privacy issue, or a billing dispute. |

Coverage includes Austrian, German, EU, Schengen cross-border, US federal and US state law,
including regional codes. The server lists the jurisdictions it currently covers when your
agent connects, so that list is always current. Which jurisdictions are included and which
are add-ons is listed at https://legal.maxhealth.tech/pricing.

## Sign-in and data

You sign in with OAuth the first time the agent calls the server. Nothing is stored in
the plugin itself. Your questions and the tool calls they produce go to
`legal.maxhealth.tech`, which fetches the matching law and case law from the official public
sources. Details are in the [privacy policy](https://legal.maxhealth.tech/privacy).

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install legal-research@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add legal-research@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./legal`.

Any other MCP client:

```json
{
  "mcpServers": {
    "legal": { "url": "https://legal.maxhealth.tech/mcp" }
  }
}
```

## Try it

- "What does § 1295 ABGB require for a damages claim?"
- "My insurer denied an MRI my doctor ordered in California. What are my appeal rights and deadlines?"
- "Compare the GDPR access right with the Austrian DSG."

## Layout

```
legal/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
└── skills/
    ├── legal-research/SKILL.md
    └── patient-rights-advocate/SKILL.md
```

More at https://legal.maxhealth.tech.
