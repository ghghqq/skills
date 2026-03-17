# CIP Market Front H5 Adapter

Use this reference only when the current repository is the CIP marketing mobile workspace with packages such as `mkt-group-forth`, `mkt-group`, and `mkt-common`.

## Repository Shape

This workspace is a Yarn monorepo centered around:

- `packages/mkt-group`: group mobile app.
- `packages/mkt-group-forth`: fourth-bureau mobile app.
- `packages/mkt-common`: shared components and helpers.

For UDP mobile feature work in this repository, the most relevant business package is usually `packages/mkt-group-forth`.

## Documents Worth Reading

When present, start from these repository documents:

- `packages/mkt-group-forth/README.md`: project-local development notes and multi-form guidance.
- `packages/mkt-group-forth/UDP旧功能重开发步骤记录.md`: migration workflow for rebuilding legacy features on UDP.
- `packages/mkt-group-forth/UDP平台移动端开发指南.md`: generated UDP page structure, controller patterns, and common capabilities.

If the task touches a known module, also read its design notes and any nearby `TXF-NOTE`, `TXF-TODO`, or `TXF-QUESTION` anchors before editing.

## Verified Commands

Repository-level commands:

- `yarn dev-forth`
- `yarn build-forth`
- `yarn dev-group`
- `yarn build-group`

Workspace-level commands that are verified from package scripts:

- `yarn workspace @cipmkt/mkt-group-forth run dev`
- `yarn workspace @cipmkt/mkt-group-forth run build`
- `yarn workspace @cipmkt/mkt-common run dev`
- `yarn workspace @cipmkt/mkt-common run build`
- `yarn workspace @cipmkt/mkt-common run lint`

## Validation Guidance

- There is no unified root `lint`, `typecheck`, or `test` command.
- Prefer the affected package command when it exists.
- `mkt-group-forth` exposes `build` but not a dedicated `lint` script, so build plus targeted manual checks may be the best available validation.
- Historical root scripts mention `mkt-base`, but that workspace is not part of the visible repository layout. Do not rely on those scripts unless the workspace is confirmed.

## Working Conventions In This Repository

- Follow repository instructions before editing, especially local `AGENTS.md` and commit rules.
- Search nearby `TXF-*` anchors before modifying related files, then update them when the change affects important or confusing logic.
- Keep changes focused on the target package and target module.
- Do not assume all UDP pages use the same naming style. Confirm the real module path and casing before patching.

## Practical Defaults

- Start from JSON and page hooks before changing generic shared components.
- Use `packages/mkt-group-forth/src/pages` as the first search area for fourth-bureau mobile page work.
- When a feature lacks automated checks, document the exact manual verification scope in the final report.
