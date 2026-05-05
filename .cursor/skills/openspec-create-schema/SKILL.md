---
name: openspec-create-schema
description: Scaffold a new custom OpenSpec schema inside the byond-openspec-schemas repository. Generates schema.yaml with artifact dependencies, starter templates for each artifact, an example config.yaml, and updates the README. Use when the user wants to add a new OpenSpec workflow/schema to the public Beyond schemas repository, or asks to create or contribute a new OpenSpec schema.
disable-model-invocation: true
---

# Create OpenSpec Schema

Use this skill to add a new custom OpenSpec schema to the `byond-openspec-schemas` repository. The skill creates the file structure and updates the README. The user reviews, commits and opens the PR.

## Pre-flight checks

Run these checks before doing anything else.

1. **Confirm working directory.** Check that `schemas/` and `examples/` folders exist at the repo root. If not, stop and tell the user:
   > This skill must run from the root of a clone of `byond-openspec-schemas`. Run `git clone https://github.com/byndcloud/byond-openspec-schemas` and try again from inside the clone.

2. **Suggest a fresh branch.** Run `git status --short --branch` to see the current branch. If on `main`, suggest:

   ```bash
   git checkout -b add-<schema-name>-schema
   ```

3. **Check name collision.** If `schemas/<name>/` already exists, ask the user whether to bump the schema's `version` (edit existing) or pick a different name. Do not silently overwrite.

## Discovery

Use AskUserQuestion when available. Gather:

| Field | Format | Examples |
| --- | --- | --- |
| `name` | kebab-case, lowercase | `event-driven`, `minimalist`, `bug-fix-fast` |
| `description` | one sentence | "Lean schema for low-risk fixes." |
| `artifacts` | ordered list of ids | `specs, tasks` &middot; `events, specs, design, tasks` &middot; `prd, sdd, bdd, tdd, code, mutation, review` |
| `dependencies` | which artifact requires which | Default: each requires the previous one (linear chain) |
| `apply.requires` | the final implementation artifact | Usually `tasks` or `code` |

If unclear, read `schemas/full-cycle-sdlc/schema.yaml` and treat it as the reference example.

## Scaffold

Create the following files. Replace `<name>` with the schema name and `<artifact>` with each artifact id from the discovery step.

### 1. `schemas/<name>/schema.yaml`

```yaml
name: <name>
version: 1
description: <description>
artifacts:
  - id: <artifact-id>
    generates: <artifact-id>.md
    description: <one-line description>
    template: <artifact-id>.md
    instruction: >
      <2-3 sentences explaining what this artifact must contain
      and what constraints apply. Reference previous artifacts
      explicitly when applicable.>
    requires:
      - <dependency-id>

apply:
  requires:
    - <final-implementation-artifact>
  tracks: tasks.md
  instruction: |
    <Implementation guidance for the apply phase. Name which
    artifacts must exist before code can start.>
```

Rules:

- The first artifact in the list has `requires: []`.
- `apply.requires` must list the artifact that contains the checkbox tasks (usually `tasks` or `code`).
- `apply.tracks` is the markdown file the apply command parses for `- [ ]` checkboxes.

For full `schema.yaml` semantics, see [reference.md](reference.md).

### 2. `schemas/<name>/templates/<artifact>.md`

Create one template per artifact id. Each template is a markdown skeleton with:

- Sections relevant to the artifact's purpose (e.g. for a PRD: Problem, Goals, Non-Goals, Acceptance Criteria, Open Questions).
- A `## Judge Gate` section at the bottom listing the criteria the user must satisfy before considering this artifact done.

Start by copying a similar template from `schemas/full-cycle-sdlc/templates/` and trimming what doesn't fit. Do **not** copy stack-specific phrasing (Supabase, pgTAP, Vitest, etc.) unless your schema explicitly assumes that stack.

### 3. `examples/<name>/config.yaml.example`

```yaml
schema: <name>

context: |
  Descreva aqui a stack e as convenções do seu projeto.

rules:
  <artifact-id>:
    - <rule 1>
    - <rule 2>
```

Add a `rules` block for each artifact, with 2-4 rules each. Keep rules behavior-oriented (what the artifact must include or avoid), not stack-specific unless necessary.

## Update README

Add one row to the "Schemas disponíveis" table in the root `README.md`:

```md
| `<name>` | <one-line description> | `<artifact1> → <artifact2> → ...` |
```

Keep existing rows untouched. Keep column order: name, purpose, artifacts.

## Verify

Before reporting done:

1. List the created files and confirm they are non-empty:
   - `schemas/<name>/schema.yaml`
   - `schemas/<name>/templates/<artifact>.md` (one per artifact)
   - `examples/<name>/config.yaml.example`
2. Re-read `schema.yaml` and confirm `artifacts` and `apply.requires` are coherent (every `requires` references a real artifact id).
3. Re-read the README and confirm the new row is present and the table still renders.

If the user has another local OpenSpec project, offer to copy the schema there and run `openspec status` to validate the schema parses. Do this only if the user confirms.

## Hand off

Print this summary to the user (fill the placeholders):

```text
Schema `<name>` created in:
- schemas/<name>/schema.yaml
- schemas/<name>/templates/ (<n> files)
- examples/<name>/config.yaml.example
README.md updated.

Next steps:
1. Review the files (especially schema.yaml dependencies and templates).
2. Test the schema in a real project:
     cp -r schemas/<name> /path/to/project/openspec/schemas/<name>
     # set schema: <name> in /path/to/project/openspec/config.yaml
     openspec status
3. Commit and open a PR:
     git add schemas/<name> examples/<name> README.md
     git commit -m "feat: add <name> schema"
     git push -u origin HEAD
     gh pr create --fill
```

## Guardrails

- Do **not** run `git add`, `git commit`, `git push`, or `gh pr create`. The user does that.
- Do **not** modify existing schemas or their templates.
- Do **not** skip the README update.
- Do **not** copy stack-specific language from `full-cycle-sdlc` templates without explicit user intent.
- If the user wants a schema with the same name as an existing one, ask whether to bump `version` or pick a different name.

## Reference

For `schema.yaml` semantics, artifact dependency rules and template patterns, see [reference.md](reference.md).
