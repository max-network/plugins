# Kallikrat

Construction plans from the conversation. Describe a building and your agent draws it as an
IFC model, shows it as plan sheets or in 3D, records what was built differently on site, and
issues revisions as PDF, IFC and BCF.

## What's inside

| Component | Purpose |
| --- | --- |
| `kallikrat` MCP server (`https://kallikrat.com/mcp`) | Creates and edits plans as IFC models (walls, rooms, doors, windows, slabs, roofs, stairs, terrain, materials), imports PDF plans and photos of paper plans, records as-built deviations, issues revisions and exports PDF, IFC and BCF. Renders plan sheets and an interactive 3D view through the [MCP Apps](https://modelcontextprotocol.io/seps/1865-mcp-apps-interactive-user-interfaces-for-mcp) extension. |
| `kallikrat-plans` skill | Teaches the agent the order of calls for authoring a plan and for as-built documentation, to work in millimetres, to ask for missing measurements instead of guessing them, and to confirm assumed values before a revision is issued. |

Measurements can be said, typed, or read from an uploaded sketch, photo or PDF plan. A photo of
a paper plan can also be taken with a phone through a short-lived upload link. The server lists
its tools when your agent connects.

## Sign-in and data

You sign in and approve access the first time the agent calls the server. Plans, sheets, the
PDFs and photos you upload and the deviations you record go to `kallikrat.com` and are scoped
to your account.

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install kallikrat@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add kallikrat@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./kallikrat`.

Any other MCP client:

```json
{
  "mcpServers": {
    "kallikrat": { "url": "https://kallikrat.com/mcp" }
  }
}
```

## Try it

- "Create a floor plan for a single-storey house, 12 by 9 metres, with 30 cm outer walls."
- "Here is a photo of the paper plan. Add it to the Berger project as revision C."
- "The kitchen window was built 20 cm wider than planned. Record that."
- "Show the plan in 3D and hide the roof."

## Layout

```
kallikrat/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
├── assets/icon.png
└── skills/kallikrat-plans/SKILL.md
```

More at https://kallikrat.com.
