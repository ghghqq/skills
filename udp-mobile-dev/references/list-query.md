# UDP List Query Reference

Use this reference when a UDP mobile task touches top keyword search, QueryPanel filters, or backend `condition` payload assembly.

## Decision Shortcut

1. Only changing which field the top search input queries?
   - Change module JSON `list.searchBar.searchKey` first.
   - Verify `list/index.tsx` sends `{ [searchKey]: keyword }`.
2. Changing advanced filter fields or a scheme-driven query panel?
   - Check `queryPanel` config, query scheme ownership, or QueryDropDown behavior.
3. Changing backend payload shape or compare handling?
   - Change the shared query utility or `list/hooks.tsx` where `condition` is assembled.

## Runtime Flow

1. `list/index.tsx`
   - Top Search uses `searchKey` from JSON.
   - QueryPanel or QueryDropDown returns advanced filter values.
2. `list/hooks.tsx`
   - Merges default conditions and runtime query values.
   - Converts the query object into backend `condition` items.
3. Shared utility
   - Reuse a common utility when multiple pages need the same query normalization.

## Normalization Rules

- Convert snake_case database names in composite query keys to camelCase backend fields when the backend expects entity-field names.
- Map compare operators explicitly.
  - `like` -> `CN`
  - `in` -> `IN`
  - `between` -> `BT`
- Stop and clarify before supporting a new compare operator.
- Convert `date` values to timestamps.
- If compare is `IN` and the data is scalar, wrap it into a single-element array.
- Keep default conditions separate from per-request user query values.

## Validation Checklist

- Inspect the emitted query object from the search UI.
- Confirm the final `condition` array shape before request dispatch.
- Verify:
  - field names
  - compare values
  - date timestamps
  - `IN` array payloads
  - preserved default conditions

## Common Mistakes

- Editing hook-level compare logic when the real need is only to change `list.searchBar.searchKey`.
- Treating top Search and QueryPanel as the same path.
- Duplicating query normalization in multiple list pages instead of reusing a shared util.
- Silently passing through unsupported compare operators.
