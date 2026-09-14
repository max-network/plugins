---
name: dry-codes
description: Reuse existing code instead of writing it twice. Use BEFORE implementing any new function, component, utility, type, endpoint, hook, or module — query the DRY.codes MCP to find an existing implementation to reuse, and check for near-duplicates before finishing. Also a searchable knowledge base over code AND docs (READMEs, ADRs, conventions) — search it for prior decisions, naming, and patterns before writing so you stay consistent. Chain several tool calls (search, then read, then compare, then check) rather than a single lookup. Triggers when about to write new code, "implement X", "add a helper/util", "create a component", refactoring toward reuse, or whenever following DRY (Don't Repeat Yourself).
interface:
  display_name: "DRY.codes"
  short_description: "Reuse before you write — search indexed code & docs."
  icon_small: "./assets/icon.png"
  icon_large: "./assets/logo.svg"
  brand_color: "#3fb950"
---

# DRY.codes — reuse before you write

You have access to the **DRY.codes** MCP server, a searchable knowledge base over
indexed repositories. It indexes both **code** and **docs** (READMEs, ADRs, guides)
and finds them by text AND by meaning. Your job is to *reuse what already exists*
(code, conventions, and decisions) instead of regenerating it, and to keep the
codebase free of duplicates and inconsistencies.

DRY stands for Don't Repeat Yourself. The opposing view is WET (Write Everything Twice).
The counter-balance is AHA (Avoid Hasty Abstractions): abstract only on real, repeated
evidence, never on a single guess.

## Connect the server, then follow what it advertises

On first use this plugin signs you in (GitHub OAuth) and lets you pick which corpus to
search: one of **your own** DRY.codes MCP endpoints, or the **public corpus** (public
repos shared on DRY.codes). To switch later, reconnect and pick another. Manage your
endpoints at https://dry.codes.

On connection the server advertises **the tools this endpoint exposes and how to use
them** — the roster, the search tactics, and the scopes available to you. That list
adapts to the endpoint and the account behind it, so **follow what it says rather than
assuming a tool exists**. Read its instructions before your first call, and use whatever
it exposes. If you are unsure what is available, ask the server rather than guessing at
a tool name.

## The cardinal rule

Before writing any non-trivial function, component, type, endpoint, or module, **search
here first** for an existing implementation to reuse. Before you finish, **check that you
did not add a near-duplicate**.

## Chain your calls — one lookup is rarely enough

A single search tells you little. The value comes from **chaining** tools: cast a wide
net, read the strongest hits, then CONFIRM with a second tool before you act. Treat each
result as a lead for the next call, not a final answer. Fire independent searches in
parallel. If the first call comes back thin, widen it — loosen the threshold, ask for more
results, drop a filter, search the docs instead of the code — before concluding nothing
exists. That conclusion after one or two calls is almost always wrong.

The shape of a reuse decision is always the same:

1. **Search** for an existing implementation, by text and by meaning.
2. **Read** the strongest hits. Every search answers in paths and line numbers, so a hit
   only becomes a decision once you have read the source. Never judge a file by its path.
3. **Reuse** the winner — import, call, extend, or adapt it — instead of writing a parallel
   version, and cite the file you reused.
4. **Stay consistent** with the naming, structure, and patterns you found, so you do not
   introduce a second style for the same thing.
5. **Check before you finish** that you did not add a near-duplicate.

Search the **docs**, not just the code, before you decide anything: an ADR, README, or
convention may already settle the question, and following it is cheaper than re-deciding.

## Guidelines

- Reuse beats regenerate: it spends far fewer tokens and keeps the codebase DRY.
- Prefer extending an existing utility over creating a second one that does the same job.
- Only write new code when nothing suitable exists, then put it in ONE place so the next
  agent can reuse it too.
- Do not stop at the first search. Chain a confirming call before you conclude.

## When duplication spans repositories

DRY.codes sees duplication ACROSS repos, not just within one. That is the signal for
*abstraction* and *consolidation*, not just local reuse. Mind AHA: abstract only when the
evidence is real and repeated.

When the same thing is duplicated in several repos, **propose extracting it into one
shared place** (a shared package, library, or repository each consumer depends on) instead
of editing the copies in place. Name every repo that would benefit, using the duplication
results as evidence.

Treat this as a **recommendation**: surface it, explain the trade-off, and let the user
decide. Do NOT create a new repository or restructure projects without their go-ahead.

## Examples

- "Add a function to format dates" → search for an existing date formatter → read the hit
  → reuse it if found.
- "Create a CSV parser" → search for a parser, then compare the two closest before writing one.
- "Follow our API conventions" → search the docs for the ADR before coding.
- "This util appears in three of our repos" → run the duplication views → confirm the
  strongest pairs → propose a shared package.
- Wrapping up a change → run the duplication views again to catch duplicates you may have
  introduced.

Learn more: https://dry.codes
