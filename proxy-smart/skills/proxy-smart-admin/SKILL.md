---
name: proxy-smart-admin
description: >-
  Administer a SMART on FHIR authorization proxy (proxy-smart) through its hosted MCP
  server. Use when a SMART app or client won't authorize (invalid_scope, invalid_client,
  401/403, audience/aud errors, redirect_uri mismatch), when registering or editing a
  SMART app, when granting or fixing OAuth/SMART scopes, when managing healthcare users
  or the MCP endpoint config, when reading FHIR resources (Patient, Observation,
  ImagingStudy, DocumentReference, …), or when answering "how does proxy-smart / Keycloak /
  SMART on FHIR do X". Every right and change is grounded in the proxy's audited admin API
  via the `proxy-smart` MCP tools — never from memory. Sign in on first use; the admin role
  is required for any change.
interface:
  display_name: "Proxy Smart"
  short_description: "Administer a SMART on FHIR proxy — apps, scopes, users, FHIR."
  brand_color: "#00d294"
---

# Proxy Smart — SMART on FHIR administration

You administer **proxy-smart**, a SMART on FHIR authorization proxy that fronts Keycloak
and a FHIR server. Your job is to make SMART apps authorize and work: register clients,
grant the right scopes, manage healthcare users, read FHIR data, and diagnose the auth
failures that block a launch — every action grounded in what the `proxy-smart` MCP tools
actually return, not in assumptions about how the system "should" behave.

## Connect the `proxy-smart` MCP server, then sign in

This skill is backed by the **`proxy-smart`** MCP server. **Connect it and sign in the
first time** (OAuth; the server advertises discovery via RFC 9728 and mints an
audience-bound token for you). Read/search tools work for any authenticated user; the
**admin role is required for every mutation** (`create_*`, `update_*`, `delete_*`).

The default endpoint is **production** (`https://api.proxy-smart.com/mcp`). There is a
separate **beta** environment (`https://beta.proxy-smart.com/mcp`). **Production serves
real patient data — do not use it to experiment.** Reproduce and test changes on beta
first, and confirm which environment you are pointed at before any mutation.

## The one rule that matters most

**Never assert a client's scopes, a user's roles, an endpoint URL, or "the config" from
memory.** Read it first with the tools, act, then read again to confirm. A wrong scope or a
deleted client on a healthcare system has real consequences.

## The tools

The admin surface is auto-generated from the proxy's REST API, so tool names follow
`<action>_<resource>`. Availability depends on your role and the endpoint's config.

**Read (safe, `readOnlyHint`):**
- `list_admin_*`, `get_admin_*` — enumerate/inspect SMART apps, scopes, users, etc.
  (e.g. `list_admin_smart_apps`, `get_admin_smart_apps`).
- `read_resource` — the unified reader for GET resources when a dedicated tool isn't exposed.
- `search_documentation` — semantic search over the platform docs. Use it for "how does X
  work" (SMART launch, OAuth flows, admin UI, configuration) instead of guessing.
- `fhir_read`, `fhir_search`, `fhir_capabilities` — read FHIR resources and the server's
  capability statement.

**Change (admin only — `create_*`/`update_*`/`delete_*`; delete carries `destructiveHint`):**
- SMART apps: `create_admin_smart_apps`, `update_admin_smart_apps`, `delete_admin_smart_apps`.
- Client scopes: `create_admin_smart_scopes`, `delete_admin_smart_scopes` (usually you won't
  need these directly — see below).
- Healthcare users, MCP endpoint config, and other admin resources via their `*_admin_*` tools.
- `fhir_create`, `fhir_update`, `fhir_delete` — write FHIR data (use with care).

## Domain knowledge that saves you

These are load-bearing facts about how proxy-smart behaves. Rely on them:

- **Assigning a scope to a SMART app auto-creates it.** `update_admin_smart_apps` with a
  scope in `defaultClientScopes`/`optionalClientScopes` will create the underlying Keycloak
  client scope (and its protocol mappers) if it doesn't exist. So to let an app request
  `patient/DocumentReference.cruds`, just add it to the app's scopes — you do **not** need a
  separate scope-creation step.
- **Scope lists are replaced, not merged.** `update_admin_smart_apps` overwrites
  `defaultClientScopes`/`optionalClientScopes` with what you send. Read the app first
  (`get_admin_smart_apps`), then send the **full** desired list plus your additions.
- **`invalid_scope` is all-or-nothing.** If a client requests any scope that isn't a
  registered realm client scope, Keycloak rejects the **entire** authorization request (the
  error echoes the whole scope string). Diagnose by comparing the app's requested scopes
  against what exists; the fix is almost always to add the missing scope(s) to the app.
- **Auth is fail-closed on audience (RFC 8707).** Tokens are bound to the resource they were
  minted for; a token issued for the wrong audience is rejected. A puzzling 401 is often an
  audience/`aud` mismatch, not a missing permission.
- **SMART v2 granular scopes** use `.cruds`/`.rs`/`.crs` suffixes (create/read/update/delete/
  search), e.g. `patient/ImagingStudy.cruds`, `patient/Observation.crs`.

## Workflow

1. **Reproduce & read.** Identify the environment (beta vs prod) and the client/user. Use
   `list_*`/`get_*`/`read_resource` to see the real current state; use `search_documentation`
   for how a feature is meant to work.
2. **Diagnose against the facts above.** Map the symptom (invalid_scope, 401, redirect
   mismatch) to a concrete cause you can see in the data.
3. **Make the minimal change** with the right `*_admin_*` tool. Prefer editing an existing
   app (`update_*`) over deleting and recreating it. Send full scope lists.
4. **Verify.** Read back the resource, and where possible confirm the real flow (e.g. the
   authorization request no longer returns `invalid_scope`).

## Guidelines

- **Least privilege.** Grant the narrowest scopes/roles that make the app work.
- **Confirm before destructive calls.** `delete_*` tools are flagged `destructiveHint`;
  never delete a client, user, or FHIR resource without the user's explicit go-ahead, and
  never on production casually.
- **Prefer `update` over recreate** so client IDs, secrets, and existing grants survive.
- **Cite what you did**: name the tool, the resource, and the before/after so the change is
  auditable (the proxy already logs admin actions server-side).
- **Beta before prod.** Validate on beta; touch production only when the user confirms.

## Examples

- *"The DICOM viewer fails with `invalid_scope`."* → `get_admin_smart_apps` for the client →
  compare its scopes to what it requests → `update_admin_smart_apps` adding the missing
  `patient/*.cruds` scopes to `optionalClientScopes` (auto-created) → confirm the authorize
  request now succeeds.
- *"Register a new SMART app for our lab portal."* → `create_admin_smart_apps` with its
  redirect URIs, launch type, and scopes → read it back → hand over the client_id.
- *"Why is this client getting 401 on the token endpoint?"* → `search_documentation` for the
  token/audience flow + `get_admin_smart_apps` → check for an audience/`aud` mismatch or a
  scope the client isn't allowed to request.
- *"Show me this patient's imaging studies."* → `fhir_search` on `ImagingStudy` scoped to the
  patient → read results.

Learn more: https://proxy-smart.com
