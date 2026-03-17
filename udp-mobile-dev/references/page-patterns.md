# UDP Mobile Page Patterns

Use this reference when modifying an existing UDP mobile page or reviewing where a change should live.

If the task is mainly about search, filter, or backend condition payloads, also read `references/list-query.md`.

## Stack Assumptions

Typical UDP mobile pages in this environment use:

- React with TypeScript.
- JSON-driven page metadata.
- UmiJS-style routing.
- Dva-style store structure.
- UDP request and external action helpers.

## Standard Module Shape

```text
module/
|- module.json
|- list/
|  |- index.tsx
|  |- controller.ts
|  |- hooks.tsx
|  |- service.ts
|  |- store.ts
|  |- card.tsx
|  `- types.ts
`- detail/
   |- index.tsx
   |- controller.ts
   |- hooks.ts
   |- service.ts
   |- store.ts
   |- form.tsx
   `- grid.tsx
```

## Where Changes Usually Belong

### JSON

Use JSON for:

- Buttons and toolbar structure.
- Endpoint configuration.
- List columns.
- Simple keyword search settings such as `list.searchBar.searchKey`, `showSearchBar`, `filterType`, and `outQueryNum`.
- Detail field groups.
- Tab composition.

Prefer JSON if the change is declarative and stable.

### List Page

Use `list/controller.ts` for:

- Add, edit, delete, submit, and history click handlers.
- Button-specific loading behavior.
- Status-based action decisions.

Use `list/hooks.tsx` for:

- Query parameter assembly.
- Default conditions and default sorting.
- Mapping search UI state into request payloads.

Use `list/index.tsx` plus JSON together for:

- Top keyword search behavior that reads `list.searchBar.searchKey` and sends `{ [searchKey]: keyword }`.
- Distinguishing simple Search input changes from QueryPanel advanced filter changes.

Use `list/service.ts` for:

- Calling list and delete APIs.
- Translating backend response fields into page-friendly `list` and `total` structures.

Use `list/card.tsx` for:

- Display-only formatting.
- Converting object-like values into plain text.
- Mapping tags, percentages, or date display.

### Detail Page

Use `detail/hooks.ts` for:

- Building form configs from JSON.
- Building grid configs from JSON.
- Translating language labels.
- Injecting runtime-only props such as `value_key`, attachment settings, or conditional groups.

Use `detail/service.ts` for:

- Detail requests.
- Save requests.
- Auxiliary data lookups.
- Reference or prefill requests.

Use `detail/controller.ts` for:

- Save.
- Attachment entry.
- Submit and flow actions.
- Back-navigation or action orchestration.

Use `detail/form.tsx` and `detail/grid.tsx` only for view-layer behavior that cannot stay inside generic components.

## Data Handling Rules

- Normalize backend response shapes once in service files.
- Keep display formatting close to render components.
- Keep business branching in hooks or controllers, not inline in JSX when avoidable.
- Use early returns for state-dependent branches.

## Status Rules

State handling often decides whether a row can be edited, deleted, or submitted.

Recommended pattern:

1. Identify the authoritative status fields.
2. Write one small rule set for allowed actions.
3. Reuse that rule set anywhere button state or action guards are needed.

Do not duplicate slightly different status logic in multiple files.

## Attachment And Flow Notes

- Attachment behavior depends on a stable main record identity.
- Flow actions require confirmed business identifiers before wiring handlers.
- Treat attachment and flow integration as late-stage work after basic data loading is stable.

## Anchor-Friendly Composition

When the page needs anchor navigation, keep forms and tables as separate configurations so the page shell can compose them into navigation blocks without special one-off markup.
