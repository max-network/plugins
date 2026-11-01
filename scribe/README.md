# Scribe by Max Health

Say it once, keep the record. Describe how you are feeling, or hand over a discharge summary, a
lab report or a letter, and get back structured clinical records someone can check before
anything is saved.

## What's inside

| Component | Purpose |
| --- | --- |
| `scribe` MCP server (`https://scribe.maxhealth.tech/mcp`) | Turns a clinical narrative in any language, or a clinical PDF, into FHIR R4 resources, validates each against its [IPS](https://hl7.org/fhir/uv/ips/) profile and verifies every code against a terminology server. |
| `clinical-scribe` skill | Teaches the agent to take this down without overstating it: codes are never invented, the person's own words survive, and what comes back is read to them before it counts. |

Nothing is saved. The server returns a proposal and stops; whatever reaches a medical record is
written by the caller, through their own client, from what the person accepted. Anything read
out of a document keeps that document attached as its source. The server lists its tools when
your agent connects.

## Sign-in and data

You sign in with your Max Health account the first time the agent calls the server. The text or
document you hand over goes to `scribe.maxhealth.tech`, which sends it to the Max Health model
gateway (`llm-gateway.maxhealth.tech`) to draft the records and looks codes up on the Max Health
terminology server (`tx.maxhealth.tech`). A PDF without a text layer is read with Cloudflare
Workers AI. Model use is metered against your Max Health plan. Details are in the
[privacy policy](https://maxhealth.tech/privacy).

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install scribe@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add scribe@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./scribe`.

Any other MCP client:

```json
{
  "mcpServers": {
    "scribe": { "url": "https://scribe.maxhealth.tech/mcp" }
  }
}
```

## Try it

- "I've had a dry cough for about two weeks and my temperature was 38.2 this morning. Write that down for my record."
- "Here's my discharge summary. Pull out the diagnoses and medications so I can check them."
- "Read this lab report and draft the observations."

## Layout

```
scribe/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
├── assets/icon.png
└── skills/clinical-scribe/SKILL.md
```

More at https://scribe.maxhealth.tech.
