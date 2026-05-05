---
name: openspec-update-schema
description: Update an existing OpenSpec schema in byond-openspec-schemas. Handles adding, removing or renaming artifacts, changing dependencies between artifacts, updating templates or instruction text, changing apply.requires, and version bumps for breaking changes. Use when the user wants to modify an existing schema in the public Beyond schemas repository, or asks to update, extend, fix, refactor or migrate an OpenSpec schema.
disable-model-invocation: true
---

# Update OpenSpec Schema

Use this skill to modify an existing schema in `byond-openspec-schemas`. The skill edits files in place and updates the README. The user reviews, commits and opens the PR.

## Pre-flight checks

1. **Confirm working directory.** Check that `schemas/` and `examples/` folders exist at the repo root. If not, stop and tell the user to clone `byond-openspec-schemas` and run from inside the clone.

2. **Suggest a fresh branch.** Run `git status --short --branch`. If on `main`, suggest:

   ```bash
   git checkout -b update-<schema-name>-<short-description>
   ```

3. **List schemas.** If the user did not specify which schema to edit, run:

   ```bash
   ls schemas/
   ```

   and ask the user which one.

## Discovery

Ask the user (use AskUserQuestion when available):

1. **Which schema** to update? (must match a folder under `schemas/`)
2. **What kind of change**? Pick one or more:
   - Add a new artifact
   - Remove an artifact
   - Rename an artifact
   - Change `requires:` of an existing artifact
   - Update a template (`templates/<id>.md`)
   - Update `instruction:` of an artifact
   - Change `apply.requires`
   - Change `description:`
   - Other

Then read the current state:

- `schemas/<name>/schema.yaml`
- The matching row in `README.md` "Schemas disponíveis"

Show the user a summary of what is currently there before applying changes.

## Apply changes

Pick the subflow that matches the change type. Multiple subflows can run in one invocation.

### Add artifact

1. Append to the `artifacts:` list in `schema.yaml`. Required fields: `id`, `generates`, `description`, `template`, `instruction`, `requires`.
2. Create `schemas/<name>/templates/<id>.md` with sections relevant to the artifact's purpose, ending in `## Judge Gate`.
3. Re-check whether `apply.requires` should include the new artifact. Ask the user.

### Remove artifact

This is **breaking**. Bump `version`.

1. Remove the artifact entry from `artifacts:`.
2. Delete `schemas/<name>/templates/<id>.md`.
3. For every other artifact whose `requires:` referenced this id, update the chain (ask the user how to reroute).
4. If `apply.requires` referenced this id, update it.

### Rename artifact

This is **breaking**. Bump `version`.

1. Update `id`, `generates` and `template` in the artifact entry.
2. Rename `schemas/<name>/templates/<old>.md` to `schemas/<name>/templates/<new>.md`.
3. Update every `requires:` that referenced the old id.
4. Update `apply.requires` if applicable.

### Change `requires` of existing artifact

This is **breaking**. Bump `version`.

1. Edit `requires:` of the artifact.
2. Validate the resulting graph: no cycles, all referenced ids exist.

### Update template

1. Open `schemas/<name>/templates/<id>.md`.
2. Apply the user's edits.
3. Confirm the file still ends with a `## Judge Gate` section.

### Update `instruction`

1. Edit the `instruction:` field of the artifact in `schema.yaml`.
2. Keep it 2-4 sentences. No version bump needed.

### Change `apply.requires`

This is **breaking**. Bump `version`.

1. Edit the `apply:` block.
2. Validate every referenced id exists in `artifacts:`.

### Change `description`

1. Edit `description:` in `schema.yaml`.
2. Update the matching row in `README.md`.

## Version bump rules

After applying changes, bump `version: <int>` in `schemas/<name>/schema.yaml` if **any** of the following happened:

- Added, removed or renamed an artifact.
- Changed `requires:` of an existing artifact.
- Changed `apply.requires`.
- Description changed in a way that alters the schema's meaning.

Do **not** bump for:

- Template wording.
- `instruction:` rephrasing that does not change the contract.
- New examples in `examples/`.

If you bump version, note it in the commit-message suggestion at the end.

## Update README

Update the corresponding row in the "Schemas disponíveis" table if any of these changed:

- Description.
- Artifact list (the right-most column showing the workflow).

Do not touch other rows.

## Verify

Before reporting done:

1. Read `schemas/<name>/schema.yaml` and confirm:
   - All `requires:` reference real artifacts.
   - No cycles in the dependency graph.
   - `apply.requires` only lists existing artifacts.
2. List `schemas/<name>/templates/` and confirm 1:1 mapping with `artifacts:` (no orphan templates, no missing templates).
3. Re-read the README row and confirm it matches `schema.yaml`.

If the user has another local OpenSpec project, offer to copy the schema and run `openspec status` to validate the YAML parses. Do this only with explicit confirmation.

## Hand off

Show this summary (fill placeholders):

```text
Schema `<name>` updated:
- schemas/<name>/schema.yaml (modified)
- schemas/<name>/templates/<id>.md (added | removed | renamed | edited)
- README.md (row updated)
- Version: <bumped to N | unchanged>

Next steps:
1. Review the diff:
     git diff schemas/<name> README.md
2. Test the schema in a real project:
     cp -r schemas/<name> /path/to/project/openspec/schemas/<name>
     openspec status
3. Commit and open a PR:
     git add schemas/<name> README.md
     git commit -m "<type>(<name>): <summary>"
     git push -u origin HEAD
     gh pr create --fill
```

For breaking changes, suggest the commit prefix `feat!:` or include a `BREAKING CHANGE:` footer in the commit body. Mention which projects might need to migrate.

## Guardrails

- Do **not** run `git add`, `git commit`, `git push`, or `gh pr create`. The user does that.
- Do **not** modify other schemas in the same invocation. One schema per session.
- For breaking changes (remove/rename/change requires), confirm with the user before applying.
- Do **not** delete a schema entirely through this skill. If the user wants to remove a schema, direct them to do it manually with explicit deletion + README cleanup + a deprecation note.
- If a change would corrupt the dependency graph (cycle, missing reference), refuse and report the validation error.

## Reference

For `schema.yaml` semantics, dependency patterns and template rules, see [`docs/schema-reference.md`](../../../docs/schema-reference.md).
