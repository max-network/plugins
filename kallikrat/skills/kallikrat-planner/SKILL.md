---
name: kallikrat-planner
description: >-
  Draft and check building plans with Kallikrat, and document what was built. Use when someone
  wants to draw or change a floor plan or building model from a description, a sketch, a photo
  or a PDF, see it in 2D or 3D, survey the as-built state on site and record what was built
  differently, issue a revision, or export a plan as PDF, IFC or BCF - e.g. "draw a
  single-storey house, 12 by 9 metres, with 30 cm outer walls", "add a gable roof", "the
  kitchen window was built 20 cm wider", "show the upper floor in 3D", "issue P02".
  Backed by the `kallikrat` MCP server (kallikrat.com/mcp); sign in on first use.
---

# Kallikrat planner

Your AI drafts the plan, the user checks it in 2D and 3D. Kallikrat keeps each plan as an IFC
model with checkpoints, issued revisions and as-built surveys. You are the only language model
involved: you turn what the user says, types or shows you into tool calls, and Kallikrat checks
every change against the model. The planning decisions and the signature on a plan stay with
the people responsible for it.

## Connect the `kallikrat` MCP server, then sign in

This skill is backed by the **`kallikrat`** MCP server (kallikrat.com/mcp). The user signs in
the first time a tool is called; every project, plan and sheet is scoped to their account.
Every tool answers with one sentence plus structured data: rephrase it in the user's language.

## Never guess a measurement

- Every length is in **millimetres** (fields end in `Mm`), angles in degrees. Convert "3.6 m"
  to `3600` and "24 cm" to `240` yourself.
- When a length, thickness, height, sill, opening width or position is missing, ask for it.
  The user can say or type it, or drop a photo, a sketch, a paper plan or a PDF into the
  conversation: read it yourself and turn what you see into `create_plan` and `edit_plan` ops.
- Confirm any value you had to assume before `issue_revision`.

## Draft a plan

1. `list_sheets` to see existing projects and plans; `create_plan` for a new building with its
   storeys (name, `elevationMm`, `heightMm`) and grid.
2. `edit_plan` with batches of ops for what the user describes: walls (from, to, thickness),
   rooms (a point inside the walls), doors and windows (wall, offset, width, height), slabs,
   roofs, stairs, terrain. Chain ops in one batch with `@key`, and use `dry_run` when unsure. A
   batch is all or nothing; each stored batch is a checkpoint.
3. `describe_plan` to read back GlobalIds, `compare_revisions` to summarise what changed,
   `plan_history` to undo or restore a checkpoint.
4. `show_plan` to show a storey, or `view` 3d for the whole model.
5. `issue_revision` to issue P01, P02 and so on, then `export` as `pdf` or `ifc`.

## Conventions

- **Roof on its own storey.** By default add a storey named Roof (in the document language) at
  the top of the highest storey, tall enough for the roof, and place `add_roof` on it with
  `baseMm` 0. Put a roof on an occupied storey only when the user asks for that.
- A flat roof inside a parapet (Attika) is `add_roof` shape `flat` with `parapetHeightMm`,
  never a slab with four walls.
- A street, driveway, path or ramp is one `add_street` with its profile, added after the
  terrain; never stepped slabs or a reshaped terrain.
- Refer to elements by the GlobalIds or label ids `describe_plan` returns; the glossary
  resource explains plan abbreviations.
- **Stored sheets are kept documents, not imports.** Kallikrat never converts a drawing into a
  model; you draw the model from what you read. `add_sheet` and `request_upload` only keep a
  PDF or photo in Kallikrat, for example the issued sheet the crew documents the as-built state
  on, which then takes markup ops (`add_label`, `set_scale`, `add_note`, `add_cloud`).

## Survey the as-built state

1. `list_sheets` to find the plan; `add_sheet` or `request_upload` if the design sheet is not
   stored yet. `describe_plan` to load rooms, grid lines and tags.
2. `start_as_built` opens a survey on an issued revision. From then on the plan has two tracks:
   `edit_plan` keeps changing the design draft, `record_deviation` changes the survey.
3. On site, resolve each place the crew names, then `record_deviation` for what was built
   differently (on a model plan, as ops of what was built) and `confirm_as_planned` for areas
   built as drawn. Notes go in the project's document language, with the crew's words kept as
   the original.
4. `coverage` lists rooms still unchecked; `review_changes` with the site lead (accept, reject
   or question), then `deviation_report` to read back the result.
5. Before `issue_revision`, tell the user which rooms are unchecked. It issues the as-built
   revision and carries the design draft over onto it: report the design changes that no
   longer apply. Then `export` as `pdf` or `bcf`. `discard_as_built` closes a survey started
   by mistake.

`describe_plan`, `show_plan` and `plan_history` show the design unless `mode` is `as_built`.

## Show it, then drive the view

`show_plan` is the only tool that returns a UI, with a sheet switch, a 2D/3D switch and a
storey switch. While it is open, change it with `update_view` (2D or 3D, storey, focus,
visibility, compare planned and built, colour) instead of calling `show_plan` again; edits made
with `edit_plan` appear in it by themselves.

## Stay honest

- Report what the tools return; do not claim a change, revision or export unless the call
  succeeded.
- When a tool reports a plan limit or a read-only account, tell the user what it says and point
  to the account page it links; do not retry.
- When the user wants something the tools cannot do, say so, offer the closest workaround, and
  ask before sending it to the Kallikrat team with `submit_feedback`.

The full reference (concepts, every tool and op with its fields, worked examples) is at
https://kallikrat.com/llms-full.txt.
