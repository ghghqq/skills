---
name: udp-mobile-dev
description: Guide development, modification, and troubleshooting of UDP platform mobile pages built with React, TypeScript, and JSON-driven configuration. Use this skill whenever the user mentions UDP mobile modules, list/detail pages, module JSON, requestInfo, toolbar, grid, fieldSetForm, tabPanel, queryPanel, searchKey, controller/hooks/service/store files, attachments, flow actions, multi-form pages, or migration from older mobile pages, even if they do not explicitly ask for "UDP mobile development".
---

# UDP Mobile Dev

Use this skill to work on UDP mobile pages in a config-first way. Treat JSON as the first source of truth, keep the generated module layout stable, and push only runtime-dependent behavior into page code.

If the repository has local instructions such as AGENTS.md or commit rules, obey those before applying this skill.

## Start With Routing

1. Read local repository guidance first.
	- Check AGENTS-like instructions, commit rules, and nearby TXF anchors before editing.
2. Classify the task and load the relevant references.
	- `cip-market-front-udp-h5` repository: read `references/cip-market-front-udp-h5.md`.
	- Query, search, filter, or backend condition shaping: read `references/list-query.md`.
	- Existing page change: inspect the module JSON plus `list/` and `detail/`, then read `references/page-patterns.md`.
	- New module or legacy migration: read `references/workflow.md`.
	- `fieldSetForm` or grouped-form issue: read `references/multi-form.md`.
	- Repeated or unclear bug: read `references/troubleshooting.md` before patching.
3. Confirm the scope.
	- List, detail, or both.
	- APIs: list, detail, save, delete, submit, history, attachment, and lookup.
	- Tabs, subtables, permissions, and status fields.
	- Whether the change is declarative JSON, runtime shaping, or both.

## Working Principles

- Keep the standard UDP folder layout intact.
- Prefer JSON for stable declarative behavior:
	- `toolbar`
	- `requestInfo`
	- `grid`
	- `fieldSetForm`
	- `tabPanel`
	- simple search settings such as `list.searchBar.searchKey`
- Use `list/index.tsx` plus JSON for top keyword search behavior.
- Use hooks or shared utilities for query shaping, default conditions, runtime config derivation, and response normalization.
- Centralize shared rules such as status gating or query-condition conversion instead of duplicating them per page.
- Keep display-only formatting close to render components and backend quirks close to service or shared util layers.

## Execution Order

1. Make JSON express the baseline layout, endpoints, buttons, and simple search behavior.
2. Make the list loop stable.
	- list query
	- default conditions
	- search payload
	- open detail
3. Make detail loading stable.
	- load main data
	- render JSON-derived form config
	- add child grids after the main flow is correct
4. Add business actions.
	- save
	- delete
	- submit and history
	- attachment
5. Add status rules and special branches last.
6. Validate the affected package and document any verification gaps.

## High-Value Heuristics

- Before changing query logic, separate these paths:
	- top Search input driven by `list.searchBar.searchKey`
	- QueryPanel or QueryDropDown advanced filters
	- hook-level or shared-util payload shaping
- If the requirement is "change which field the top keyword search hits", start from JSON before touching hooks.
- If the requirement is "change what backend condition payload looks like", start from shared query utilities or `list/hooks.tsx`.
- If multiple pages need the same query transformation, extract it into the shared package instead of keeping per-page copies.
- For multi-form detail pages, treat the issue as runtime-config composition first, not JSX structure first.
- For attachment and flow actions, confirm identifiers and main-record identity before editing handlers.

## Quality Bar

- Use the repository-specific validation commands when available.
- If there is no suitable automated command, do targeted manual checks and say exactly what remains unverified.
- After solving a UDP issue:
	1. Check `references/troubleshooting.md` for an existing match.
	2. Append or update the issue log with symptom, root cause, fix, validation, and reuse notes.
	3. If the fix changed a preferred pattern, update the relevant reference file too.

## Update Discipline

Treat the troubleshooting reference as a living knowledge base. Do not leave solved issues only in chat history.

## Resource Map

- `references/cip-market-front-udp-h5.md`: repository-specific commands, docs, and validation defaults.
- `references/workflow.md`: module creation and migration order.
- `references/page-patterns.md`: file ownership and config-first placement rules.
- `references/list-query.md`: search, filter, and backend condition shaping patterns.
- `references/multi-form.md`: grouped form behavior and `fieldSetForm` pitfalls.
- `references/troubleshooting.md`: living issue log and update discipline.
