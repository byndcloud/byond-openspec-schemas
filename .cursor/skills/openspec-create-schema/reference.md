# Reference — `openspec-create-schema`

Detailed reference for the `openspec-create-schema` skill. Read only what you need.

## `schema.yaml` semantics

```yaml
name: my-schema             # required, must match folder name
version: 1                  # required, integer; bump on breaking changes
description: One sentence.  # required

artifacts:                  # required, ordered list
  - id: my-artifact         # required, kebab-case, unique within schema
    generates: my-artifact.md
    description: ...
    template: my-artifact.md
    instruction: >
      Long-form guidance shown to the AI when generating this artifact.
    requires:               # list of artifact ids this one depends on
      - other-artifact

apply:                      # required
  requires:                 # which artifacts must be done before /opsx-apply
    - tasks
  tracks: tasks.md          # which file contains the implementation checkboxes
  instruction: |
    Multi-line guidance for the apply phase.
```

Field rules:

- `id` must be unique within the schema and lowercase kebab-case.
- `generates` is the file name created when the artifact is produced. Convention is `<id>.md`.
- `template` is the file under `templates/` to use as the skeleton. Convention is also `<id>.md`.
- `requires` defines a directed acyclic graph (DAG). The OpenSpec CLI rejects cycles and missing references at validation time.
- The first artifact (in build order) has `requires: []`.
- `apply.requires` is the gating list. The CLI blocks `openspec apply` until every artifact id in this list has `status: done`.
- `apply.tracks` is the markdown file parsed for `- [ ]` and `- [x]` lines. By convention it is the same file produced by the implementation artifact.

## Dependency patterns

### Linear chain (most common)

Each artifact depends on the previous one.

```text
A → B → C → D
```

Used by `spec-driven` (proposal → specs → design → tasks) and by `full-cycle-sdlc` (prd → ... → review).

### Diamond (parallel branches)

Two artifacts depend on the same parent and are joined by a third.

```text
       A
      / \
     B   C
      \ /
       D
```

Used by `full-cycle-sdlc`: `sdd` and `bdd` both depend on `prd` and run in parallel; `tdd` requires both.

```yaml
- id: sdd
  requires: [prd]
- id: bdd
  requires: [prd]
- id: tdd
  requires: [sdd, bdd]
```

### Skipping artifacts

Use sparingly. If artifact `D` depends only on `A` (skipping `B` and `C`), declare `requires: [a]`. The CLI does not enforce a transitive chain — if you want to enforce that `D` runs after `B` and `C`, list them explicitly.

## Writing a good `instruction` field

The `instruction` is shown to the AI when it generates an artifact. Keep it 2-4 sentences.

A good instruction tells the AI:

1. **What the artifact must contain** (sections, decisions, acceptance criteria).
2. **What constraints apply** (must reference earlier artifacts, must avoid implementation details, must be readable by QA, etc.).
3. **What to skip or defer** (don't mirror existing code, don't rewrite product scope, etc.).

Bad:

> Create the design document.

Good:

> Create the Software Design Document from the PRD. Define architecture, data flow, contracts, dependencies, security and rollout. Do not introduce product scope not present in the PRD. The SDD may be created in parallel with BDD because both depend only on PRD.

## Template patterns

A template is the markdown skeleton the AI fills when generating the artifact. Use these patterns.

### Section headings, not prose

Templates should be heading-only with optional placeholders. The AI fills the body.

Bad:

```md
## Problem

Describe the problem in 2-3 sentences.
```

Good:

```md
## Problem

<!-- 2-3 sentences describing the user problem. -->
```

Or even leaner:

```md
## Problem
```

### Always end with a Judge Gate

Every artifact template must end with a `## Judge Gate` section listing the criteria the user must satisfy before treating the artifact as done.

Example:

```md
## Judge Gate

- All acceptance criteria are specific and testable.
- Business-rule references are cited or marked n/a.
- Open questions are resolved or explicitly deferred.
```

### Use `## Open Questions` when uncertainty is expected

For PRD, BDD and TDD, end the body (just before Judge Gate) with `## Open Questions`. This invites the AI to capture gaps explicitly instead of silently inventing answers.

## `config.yaml` rules block

Each artifact in the schema can have a matching `rules` block in `config.yaml`. The CLI passes these rules to the AI as constraints when generating the artifact.

```yaml
schema: my-schema

rules:
  my-artifact:
    - <constraint or quality criterion>
    - <another>
```

Rules should be:

- **Behavior-oriented**, not stack-specific. "Cite the relevant business rule" is good. "Use TanStack Query" is bad (belongs in `context:` instead).
- **Short** (one line each).
- **Imperative** ("Document RLS impacts when applicable.").
- **2-4 rules per artifact**. More than that and the AI starts ignoring them.

## Naming conventions

| Item | Convention |
| --- | --- |
| Schema folder | `kebab-case` matching `name` field |
| Artifact id | `kebab-case`, short (`prd`, `tdd`, `events`) |
| Generated file | `<artifact-id>.md` |
| Template file | `<artifact-id>.md` |
| Apply tracks file | `tasks.md` (convention) |

## Validating locally

In any project that has OpenSpec installed:

```bash
cp -r schemas/<name> /path/to/project/openspec/schemas/<name>
# Edit /path/to/project/openspec/config.yaml: set `schema: <name>`
cd /path/to/project
openspec status
```

If the schema has structural problems (missing artifact reference, cycle in `requires`, undefined apply target), `openspec status` will print the error.

## Common mistakes

| Mistake | Effect | Fix |
| --- | --- | --- |
| `apply.requires` references an artifact not in `artifacts` | `openspec status` errors | Add the artifact or fix the typo |
| Cycle in `requires` (A → B → A) | `openspec status` errors | Break the cycle; one direction only |
| Missing `template:` field | Artifact generation fails | Add `template: <id>.md` |
| Template file missing under `templates/` | Artifact generation fails | Create `templates/<id>.md` |
| Stack-specific rules in `rules:` | Schema unusable in other stacks | Move stack assumptions to `context:`, keep `rules:` behavior-oriented |
