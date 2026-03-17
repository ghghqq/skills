---
name: udp-mobile-dev
description: Guide development and maintenance of UDP platform mobile pages built with React, TypeScript, and JSON-driven configuration. Use when creating or modifying UDP mobile modules, migrating legacy mobile pages into UDP, working on list/detail pages, requestInfo/toolbar/grid/fieldSetForm/tabPanel JSON, controller/hooks/service/store wiring, attachments or flow actions, multi-form pages, or troubleshooting UDP mobile behavior.
---

# UDP Mobile Dev

Use this skill to develop UDP mobile pages in a config-first way. Keep generated module structure stable, express as much behavior as possible in JSON, and use page code only for business rules and runtime data shaping that configuration cannot cover.

If the repository has local instructions such as AGENTS.md or commit rules, obey those before applying this skill.

## Quick Routing

1. Identify the task type.
	- If the repository matches `cip-market-front-udp-h5`, also read `references/cip-market-front-udp-h5.md`.
	- New module or legacy migration: read `references/workflow.md`.
	- Existing page change: inspect the module JSON plus `list/` and `detail/`, then read `references/page-patterns.md`.
	- `fieldSetForm` or multi-form issue: read `references/multi-form.md`.
	- Repeated or unclear bug: read `references/troubleshooting.md` before changing code.
2. Keep these baseline rules.
	- Preserve the standard UDP folder layout.
	- Prefer JSON configuration over ad hoc page logic.
	- Keep API response normalization in hooks or service files, not scattered in render code.
	- Centralize state-driven button rules instead of duplicating them across components.

## Working Rules

1. Clarify the scope first.
	- Confirm whether the task touches list, detail, tabs, subtables, attachments, flow actions, or permissions.
	- Confirm the API set: list, detail, save, delete, submit, history, and attachment-related calls.
	- Confirm status fields and which actions each status allows.
2. Map the implementation surface.
	- JSON usually owns `toolbar`, `requestInfo`, `grid`, `fieldSetForm`, and `tabPanel`.
	- `list/` usually owns page entry, events, query shaping, API calls, state, and card rendering.
	- `detail/` usually owns load flow, form config shaping, grid config shaping, save flow, attachments, and submit actions.
3. Implement in the lowest-cost order.
	- Make JSON cover the baseline fields, buttons, and endpoints first.
	- Finish the list page minimal loop: open page, query list, open detail.
	- Finish detail loading and form rendering.
	- Add subtables, attachments, flow, and special state rules last.
4. Validate the affected package.
	- Run available lint or build commands for the target package.
	- If full automation is missing, do targeted manual checks and state what remains unverified.

## Decision Rules

- Use `requestInfo` as the primary API source. Hardcode endpoint defaults only when JSON is missing or incomplete.
- Keep query assembly, default conditions, and response normalization in hooks or service files.
- Use hooks to derive form and grid configuration from JSON plus runtime state.
- Keep button enablement and visibility tied to explicit status rules.
- For attachments, confirm `bindtable`, `buskey`, and the saved main record id before wiring upload or preview behavior.
- For flow actions, confirm `appCode`, `bizCode`, `dataId`, and `orgId` sources before implementation.
- When a field returns an object rather than display text, normalize it before rendering to avoid React child errors.

## Update Discipline

After solving any UDP mobile development problem:

1. Open `references/troubleshooting.md` and look for a matching issue first.
2. Append a new record with these fields:
	- Symptom
	- Root cause
	- Fix
	- Validation
	- Reuse notes
3. If the fix changes the recommended workflow or configuration pattern, update the relevant reference file too.

Treat the troubleshooting reference as a living knowledge base. Do not leave solved issues only in chat history.

## Resource Map

- `references/workflow.md`: end-to-end migration workflow and validation checklist.
- `references/page-patterns.md`: standard file responsibilities and config-first implementation patterns.
- `references/multi-form.md`: `fieldSetForm` multi-form behavior, `apiRef` usage, and common pitfalls.
- `references/troubleshooting.md`: issue log, update template, and known failure patterns.
- `references/cip-market-front-udp-h5.md`: repository-specific commands, docs, and validation guidance for the CIP marketing mobile workspace.
