# Chess by Max Health

A real chess board in the conversation. Play the built-in Stockfish engine, play the model
itself, or join a shared lobby and play live against other people. Afterwards the engine
analyses the game and the assistant explains the moments that decided it.

## What's inside

| Component | Purpose |
| --- | --- |
| `chess` MCP server (`https://chess.maxhealth.tech/mcp`) | Runs games, the live lobby and Stockfish analysis, and renders an interactive board through the [MCP Apps](https://modelcontextprotocol.io/seps/1865-mcp-apps-interactive-user-interfaces-for-mcp) extension. |
| `chess-coaching` skill | Teaches the agent to start the game and stay quiet, pick the two or three moments that decided it, and never state an evaluation that did not come from the engine. |

In hosts that support MCP Apps you move pieces by clicking the board, and inviting, accepting,
moving and resigning happen there. The server can also analyse any public chess.com or Lichess
player's recent games by handle. The server lists its tools when your agent connects.

## Sign-in and data

Playing needs no account. Signing in with Max Health adds ranked ELO and stored match history,
and lets the agent analyse your own past games. Moves, game state and lobby presence go to
`chess.maxhealth.tech`. Looking up a chess.com or Lichess handle fetches that player's public
games from those sites. Details are in the privacy policy at https://chess.maxhealth.tech/privacy.

## Install

Claude Code:

```
/plugin marketplace add max-network/plugins
/plugin install chess@max-network
```

Codex:

```
codex plugin marketplace add max-network/plugins
codex plugin add chess@max-network
```

Restart the client after installing. To try it from a checkout first, run
`claude --plugin-dir ./chess`.

Any other MCP client:

```json
{
  "mcpServers": {
    "chess": { "url": "https://chess.maxhealth.tech/mcp" }
  }
}
```

## Try it

- "Let's play chess. I'm white, put the engine on level 3."
- "Review the game we just played. Where did I lose it?"
- "What does Lichess player DrNykterstein keep getting wrong in their recent games?"

## Layout

```
chess/
├── .claude-plugin/plugin.json   Claude Code manifest
├── .codex-plugin/plugin.json    Codex manifest
├── .mcp.json                    Claude Code MCP config
├── .mcp.codex.json              Codex MCP config
└── skills/chess-coaching/SKILL.md
```

More at https://chess.maxhealth.tech.
