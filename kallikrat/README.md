# Kallikrat

Your AI drafts the plan. You check it in 2D and 3D. Describe a building, sketch it or share a
photo or PDF, and your agent drafts it as a real IFC model, from walls and windows to stairs and
roofs. Kallikrat checks every change, keeps every revision, documents what was built differently
on site, and issues revisions as PDF, IFC and BCF.

## What's inside

| Component | Purpose |
| --- | --- |
| `kallikrat` MCP server (`https://kallikrat.com/mcp`) | Creates and edits plans as IFC models (walls, rooms, doors, windows, slabs, roofs, stairs, terrain, streets, materials), keeps PDF sheets and photos of paper plans to mark up, records as-built deviations, issues revisions and exports PDF, IFC and BCF. Renders plan sheets and an interactive 3D view through the [MCP Apps](https://modelcontextprotocol.io/seps/1865-mcp-apps-interactive-user-interfaces-for-mcp) extension. |
| `kallikrat-planner` skill | Teaches the agent the order of calls for drafting a plan and for an as-built survey, to work in millimetres, to ask for missing measurements instead of guessing them, to put a roof on its own storey, and to confirm assumed values before a revision is issued. |

Measurements can be said or typed, or read by the agent from a sketch, photo or PDF plan you
drop into the conversation. A PDF or photo you want to keep in Kallikrat, such as the issued
sheet a crew documents the as-built state on, is stored as a sheet, also from a phone through a
short-lived upload link. The full reference is at https://kallikrat.com/docs.

## Sign-in and data

You sign in and approve access the first time the agent calls the server. Plans, sheets, the
PDFs and photos you upload and the deviations you record go to `kallikrat.com` and are scoped
to your account. Details are in the [privacy policy](https://kallikrat.com/privacy) and the
[terms](https://kallikrat.com/terms).

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
- "Here is a photo of the paper plan. Draw the ground floor from it."
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
└── skills/kallikrat-planner/SKILL.md
```

More at https://kallikrat.com.
