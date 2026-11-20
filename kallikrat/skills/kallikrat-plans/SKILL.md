---
name: kallikrat-plans
description: >-
  Draw and edit construction plans, and document what was built. Use when someone wants to
  create or edit a floor plan or a building model, add walls, rooms, doors, windows, slabs,
  roofs or stairs, import a PDF plan or a photo of a paper plan, show the plan in 3D, document
  what was built differently on site, issue a revision, or export a plan as PDF, IFC or BCF.
  Backed by the `kallikrat` MCP server (kallikrat.com/mcp); sign in on first use.
---

# Kallikrat plans

Kallikrat keeps construction plans as IFC models and plan sheets. You turn what the user says,
sketches or uploads into tool calls; the server draws the plan, checks every change and keeps
the revisions. Every tool answers with one sentence plus structured data: rephrase it in the
user's language.

## Connect the `kallikrat` MCP server, then sign in

This skill is backed by the **`kallikrat`** MCP server (kallikrat.com/mcp). The user signs in
and approves access the first time a tool is called. Plans, sheets and photos are scoped to
their account.

## Never guess a measurement

A plan is only as good as its numbers. Before you place anything, make sure you have:

- wall lengths (or start and end points) and thicknesses;
- room sizes, or the walls that enclose each room;
- storey heights;
- door and window widths and heights, window sills, and where each opening sits on its wall.

When one is missing, ask for it. Do not fill in a "typical" value. Ask for the few that matter
in one short question. The user answers right here in the conversation, and you do the edits:

1. **Say or type the measurements.** Put them straight into `edit_plan` ops, in millimetres:
   convert "3.6 m" to `3600` and "24 cm" to `240` yourself and say back what you understood.
2. **Drop a photo, a sketch or a PDF into the chat.** A photo of the room, a hand sketch, a
   photo of a paper plan or a PDF plan is for you to read: take the dimensions, rooms and
   openings you see into `create_plan` and `edit_plan` ops, and ask for anything you cannot
   read. Nothing needs to be uploaded to Kallikrat for this.

`add_sheet` and `request_upload` are only for keeping a document in Kallikrat, for example the
architect's PDF as the sheet to document the as-built state on, or a phone photo of a paper
plan the user wants stored as a sheet. After such a photo is stored, `describe_plan` returns
`needsTranscription: true` with the flattened image; transcribe room names, grid tags and the
scale with `edit_plan` `add_label` and `set_scale` ops (`imagePoint` is a pixel of the
flattened image).

When you had to assume anything (a wall thickness, a sill height, a door width), list those
values and have the user confirm or correct them **before** `issue_revision`. A draft can
change freely; an issued revision is a record.

## Author a plan

1. `list_sheets` to see the user's projects and plans; `create_plan` for a new building with
   its project, storeys (name, `elevationMm`, `heightMm`) and an optional structural grid.
2. `edit_plan` with a batch of ops for what the user describes: `add_wall` (from, to,
   thickness), `add_room` (`at` a point inside the walls, or a polygon), `place_door` and
   `place_window` (wall, offset, width, height, and `sillMm` for windows), `add_slab`,
   `add_roof`, `add_stair`, `add_terrain`, `set_material`. Chain ops in one batch with `@key`
   references, and use `dry_run` when unsure. Each stored batch is a checkpoint.
3. `describe_plan` to read back what exists (with GlobalIds), `compare_revisions` to summarise
   a change, `plan_history` to undo or restore a checkpoint.
4. `show_plan` to show a storey as a drawing, or `view: "3d"` for the whole model.
5. Confirm assumed values, then `issue_revision` (P01, P02 and so on) and `export` as `pdf` or
   `ifc`.

To build a model from an imported sheet, read its labels with `describe_plan` and turn the
dimensions into `create_plan` and `edit_plan` ops, asking for anything the sheet does not
show.

## Document what was built

1. `list_sheets` to find the plan; `add_sheet` or `request_upload` if the design sheet is not
   stored yet.
2. `describe_plan` to load rooms, grid lines and tags (label ids on sheets, GlobalIds on
   models).
3. `start_as_built` to open a working set on the issued revision.
4. As the crew walks the site, resolve each place they name, then `record_deviation` for what
   was built differently (batch what was said; on a model give the ops for what was built) and
   `confirm_as_planned` for areas built as drawn. Measurements are millimetres; keep the
   crew's own words as the original note. The crew can drop site photos into the chat for you
   to read; `request_upload` keeps photos attached to a deviation.
5. `coverage` to say which rooms are still unchecked; repeat step 4 until none are left.
6. `review_changes` with the site lead (accept, reject or question), then `deviation_report`
   to read back the result. Questioned deviations block issuing.
7. `issue_revision` to issue the as-built, then `export` as `pdf` or `bcf`.

## Show it, then drive the view

`show_plan` is the only tool that returns a view. Once a 3D view is open, change it with
`update_view` (focus an element, storey or deviation; show, hide or isolate storeys and IFC
types; compare planned and built; colour by material or deviation) instead of calling
`show_plan` again. Edits made with `edit_plan` appear in the open view by themselves.

## Stay accurate

- Refer to elements by the GlobalIds or label ids `describe_plan` returns, not by guesses.
- Report what the tools return. Do not claim a plan was issued, exported or changed unless the
  call succeeded.
- Abbreviations printed on plans differ by language; the server's glossary resource lists
  them.
