# UDP Multi-Form Reference

Use this reference when a UDP detail page is driven by `fieldSetForm` and may render one or more grouped forms.

## Core Model

UDP multi-form pages are configuration-driven.

The page becomes a multi-form page only when the JSON produces more than one `fieldSet` config at runtime.

Typical flow:

1. JSON defines a main `fieldSetForm` container.
2. The form hook reads that container.
3. Each `fieldSet` is converted into one form config.
4. The form renderer chooses single-form or multi-form layout based on the final form count.

## Required Configuration Rules

- Keep one main `fieldSetForm` container for the main table.
- Set `bindtable` and `buskey` correctly.
- Treat each `fieldSets` entry as one form group.
- Keep each `itemId` unique.
- Keep field `name` aligned with detail response keys.
- Use group titles consistently because they become user-visible section titles.

## Hook Responsibilities

The hook that derives form config usually handles these tasks:

- Read the selected `fieldSetForm` branch.
- Convert each `fieldSet` into a renderable form config.
- Fill multilingual labels.
- Inject shared runtime props such as `value_key`.
- Enrich attachment fields with main-table metadata.

If multiple groups do not share the same source object, extend the hook instead of forcing the JSON alone to solve it.

## Render Behavior

- One form config means a single form container.
- More than one form config means grouped form sections.
- Group rendering often drives anchor navigation or collapse panels automatically.

## Validation And Data Collection

Prefer the shared page api rather than reaching into each form manually.

Common expectations:

- `validForm` validates all grouped forms.
- `getFormData` returns data keyed by form group or item id.
- JSON-like fields may need additional validation rules based on naming or explicit logic.

## Attachment Notes

Attachment fields usually depend on the main table metadata.

Check these points when attachments fail:

- `bindtable` exists.
- The attachment field has the expected runtime metadata.
- The main record id is available when upload or preview is triggered.

## Common Failure Patterns

### The page still renders as a single form

- Only one `fieldSet` was defined.
- The hook collapsed multiple groups into one config.
- The renderer receives only one final form config.

### Fields render with blank values

- Field names do not match detail response keys.
- The wrong `value_key` is used.
- The detail data is nested under a different object than the hook expects.

### Validation or data extraction is inconsistent

- `itemId` values are duplicated.
- The page is bypassing shared `apiRef` utilities.
- Group keys are unstable between render and submit phases.

## Recommended Rule

If JSON can express the grouping, keep it in JSON. If the grouping also changes data ownership, update the hook deliberately and document the mapping.