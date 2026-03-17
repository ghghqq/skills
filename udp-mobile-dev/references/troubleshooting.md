# UDP Mobile Troubleshooting Log

Use this file as a living record of solved UDP mobile development issues.

## Update Rule

Add the newest solved issue near the top using this template:

```markdown
### YYYY-MM-DD - Short issue title

- Symptom:
- Root cause:
- Fix:
- Validation:
- Reuse notes:
```

If the fix changes the preferred development workflow, also update the relevant reference file.

## Known Issues

### 2026-03-17 - IN query compare sent a scalar instead of an array

- Symptom: A UDP query key resolved to compare `IN`, but the backend still received a scalar value and rejected or misread the condition.
- Root cause: The shared query normalization converted compare names but did not coerce non-array `IN` data into an array payload.
- Fix: In the shared UDP query utility, when mapped compare is `IN` and normalized data is not an array, wrap it as a single-element array before building the condition item.
- Validation: Confirm the built condition sends `{ compare: 'IN', data: ['value'] }` for single-value `IN` queries, and still preserves existing array input.
- Reuse notes: Treat `IN` as an array contract at the shared utility layer so page hooks do not need one-off guards.

### 2026-03-17 - Keyword search field was changed in the wrong layer

- Symptom: A page only needed to change the top search box query field, but the investigation started from QueryPanel or list hook condition conversion.
- Root cause: The simple Search component in `list/index.tsx` reads `list.searchBar.searchKey` from JSON and sends `{ [searchKey]: keyword }`, so this path is separate from advanced filter compare logic.
- Fix: Confirm whether the requirement is only to change the top keyword field. If yes, update module JSON `list.searchBar.searchKey` first instead of changing QueryPanel config or hook-level condition mapping.
- Validation: Check that the list page emits the keyword under the expected field name and that the request hook receives the updated query key.
- Reuse notes: Separate simple keyword search from advanced filter behavior before choosing the edit point; change JSON for field selection, and change hooks only when payload shaping must change.

### 2026-03-17 - UDP query object shape does not match backend condition format

- Symptom: Query components return object keys like `ca_behalf_name*str*like*1`, but the backend list API expects a `condition` array with `field`, `data`, and mapped `compare` values.
- Root cause: The page passed raw UDP query output directly into list condition assembly without normalizing field names, compare operators, or date values.
- Fix: Extract a shared utility that converts UDP query objects into backend `condition` items, camel-cases field names, maps `like -> CN`, `in -> IN`, `between -> BT`, and converts `date` values to timestamps with dayjs.
- Validation: Confirm the consuming list hook builds `condition` through the shared utility and that the affected files pass editor diagnostics.
- Reuse notes: Reuse the shared utility instead of reimplementing query parsing in each UDP list hook; add new compare mappings explicitly before supporting new operators.

### 2026-03-16 - Multi-form page does not split into multiple sections

- Symptom: A detail page expected to show multiple grouped forms still renders as one form.
- Root cause: The final runtime config contains only one `fieldSet`, or the hook collapses multiple groups into one form config.
- Fix: Check the JSON `fieldSets` count first, then verify the hook returns one config per group and preserves unique `itemId` values.
- Validation: Confirm the renderer receives more than one form config and that each group title is visible.
- Reuse notes: Treat multi-form rendering as a runtime-config problem, not a JSX composition problem.

### 2026-03-16 - Detail fields render blank after JSON changes

- Symptom: The form renders but values do not echo even though the detail API returns data.
- Root cause: Field `name` values do not match response keys, or the shared `value_key` points to the wrong source object.
- Fix: Align field names with the detail payload and update hook-level `value_key` mapping when groups read from different source objects.
- Validation: Reopen detail data and confirm each modified field echoes correctly.
- Reuse notes: Blank echo issues are usually mapping problems, not rendering problems.

### 2026-03-16 - Attachment field fails in detail page

- Symptom: Attachment upload or preview does not work, or attachment metadata is incomplete.
- Root cause: Main-table metadata such as `bindtable`, `buskey`, or saved record id is missing when the attachment field is built or used.
- Fix: Ensure the form config carries the expected attachment metadata and that attachment actions run only after the main record identity is available.
- Validation: Upload or preview an attachment from the affected detail page.
- Reuse notes: Attachment failures are often setup issues in config derivation, not UI component defects.

### 2026-03-16 - List card render crashes or shows [object Object]

- Symptom: The list page renders an object directly, causing bad display or React child errors.
- Root cause: A backend field returns an object-like value but the page treats it as plain text.
- Fix: Normalize the data in service or transform it in the card renderer before output.
- Validation: Reload the list and verify the field shows readable text without runtime warnings.
- Reuse notes: Keep object-to-text normalization out of generic page shells.

### 2026-03-16 - Edit, delete, or submit buttons behave inconsistently by status

- Symptom: Row actions are visible or clickable in states where the business flow should forbid them.
- Root cause: Status logic is duplicated or incomplete, often missing one of the controlling flags.
- Fix: Define a single rule set for action availability and reuse it anywhere button state or guards are applied.
- Validation: Check representative rows across all key statuses and confirm action behavior matches the business rule.
- Reuse notes: Centralized state rules reduce regressions during later feature additions.
