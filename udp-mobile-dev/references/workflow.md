# UDP Mobile Workflow

Use this reference when creating a new UDP mobile module or migrating a legacy mobile feature into UDP.

## Goal

Build the page in a stable order so JSON, page code, and business rules stay aligned.

## Input Checklist

Collect these items before editing code:

- Feature scope: list only, detail only, or full list-detail flow.
- Tabs and subtables: whether the page includes multiple tab pages or child tables.
- Actions: create, edit, view, delete, submit, flow history, attachment, reference data.
- APIs: list, detail, save, delete, submit, and any auxiliary lookups.
- Fields: main table, child tables, required rules, readonly rules, default values.
- States: which status values allow edit, delete, submit, or attachment changes.
- Special rules: derived fields, hidden defaults, cross-field rules, or old-page differences.

## Delivery Order

1. Create or inspect the module skeleton.
   - Keep the standard UDP layout with a module JSON plus `list/` and `detail/` folders.
2. Configure JSON first.
   - Cover `toolbar`, `requestInfo`, `grid`, `fieldSetForm`, and `tabPanel` before writing custom logic.
   - Let JSON express most layout, field, and endpoint behavior.
3. Finish the list page minimal loop.
   - Page opens.
   - List request works.
   - Search or default conditions are shaped correctly.
   - Detail page can open from the list.
4. Finish detail loading.
   - Load main data first.
   - Render forms from JSON-derived config.
   - Add child grids after main data is stable.
5. Add business actions.
   - Save.
   - Delete.
   - Submit and flow history.
   - Attachment.
6. Add state rules and special cases last.
   - Disable or hide buttons by status.
   - Handle special display branches.
   - Add derived field logic.

## JSON-First Rules

- Put baseline buttons in `toolbar`.
- Put endpoints in `requestInfo`.
- Put list columns in `grid.listgrid`.
- Put detail fields in `fieldSetForm`.
- Put tab composition in `tabPanel`.
- Only move into controller, hooks, or service files when the behavior depends on runtime branching or cannot be expressed in configuration.

## Code Responsibilities

- `list/index.tsx`: page shell and page registration.
- `list/controller.ts`: button and action logic.
- `list/hooks.tsx`: query shaping, derived list config, refresh lifecycle.
- `list/service.ts`: list and delete API calls plus response normalization.
- `list/card.tsx`: card-only rendering transforms.
- `detail/index.tsx`: page shell and component composition.
- `detail/controller.ts`: save, attachment, submit, and page actions.
- `detail/hooks.ts`: derive form and grid config from JSON plus language or runtime state.
- `detail/service.ts`: detail load, save, reference data, and auxiliary requests.
- `detail/form.tsx`: custom form rendering behavior.
- `detail/grid.tsx`: subtable rendering and row operations.

## Validation Checklist

Run or manually verify these flows in order:

1. List query.
2. Open detail.
3. Create and save.
4. Edit and save.
5. Reopen detail and verify echo.
6. Delete.
7. Submit and flow history.
8. Attachment upload or preview.
9. Child table add, edit, and delete.

## Output Expectations

- The module keeps the standard UDP directory layout.
- JSON remains the first source of truth for layout and endpoints.
- Services hide API response quirks from page components.
- Status rules are explicit and centralized.
- Verification gaps are called out clearly when automation is unavailable.