# Product Requirements Document

> Stakeholder Requirements Specification — ISO/IEC/IEEE 29148:2018

<!--
  BOOTSTRAP MODE vs DELTA MODE — check before writing.

  Bootstrap (no openspec/specs/<capability>/spec.md exists):
  - Code is the only source of truth. docs/user_stories/ are historical context.
  - Complete code investigation FIRST (see schema instruction), then fill below.
  - Record investigation findings inline in each section.

  Delta (canonical spec already exists):
  - Read openspec/specs/<capability>/spec.md as baseline.
  - Targeted code reading only for the behavior this change touches.
  - Reference canonical spec requirements by name where applicable.

  In both modes: when code and user stories diverge, code wins.
  Flag divergences as Open Questions — never resolve them silently.
-->

## Bootstrap Investigation (fill in Bootstrap mode; delete in Delta mode)

### Code Inventory

| File | Lines | Complexity hotspots |
| ---- | ----- | ------------------- |
| `<file>` | `<n>` | `<function>` CC=? / cognitive=? |

### State Map

| State | Type | Purpose |
| ----- | ---- | ------- |
| `<name>` | `useState<T>` | <what it controls> |

### Data Flow

| Query / RPC / Edge Function | Table / Name | Trigger / enabled condition |
| --------------------------- | ------------ | --------------------------- |
| `useSupabaseQuery` | `<table>` | `<condition>` |
| `supabase.rpc(...)` | `<rpc_name>` | <when called> |
| `supabase.functions.invoke(...)` | `<function>` | <when called> |

### Submit / Save Flow

1. <validation step>
2. <calculation step>
3. <persistence step>
4. <side effect>

### Baseline Tests

- Unit/integration: <X tests, Y files, green/failing>
- E2E Playwright: exists at `<path>` | **none**

### Divergences (code ≠ user story)

- <divergence or none found>

---

## Problem

<!-- What problem are we solving? Why now? -->

## Goals

<!-- User/business outcomes that must be achieved -->

## Non-Goals

<!-- Explicitly out of scope -->

## Users / Actors

<!-- Personas, roles, permissions, or systems involved -->

## Scope

<!-- In-scope product behavior, routes, modules, data, and integrations -->

## Capabilities

### New Capabilities

- `<capability-name>`: <brief description>

### Modified Capabilities

- `<existing-capability>`: <requirement-level behavior change>

## Change Classification

<!--
  This drives the E2E baseline policy below and the gates in TDD/Code/Review.
  Be honest: false negatives here cause production regressions.
-->

- Touches pre-existing code: yes | no
- Frontend (UI/UX) change: yes | no
- Business-rule change: yes | no  (refer to docs/REGRAS_NEGOCIO_COMPLETO.md and docs/user_stories/)
- Critical flow per docs/qualidade/ESTRATEGIA_ESTEIRA_QA.md §2: yes | no
- Refactor of pre-existing hotspot (codopsy grade D/E/F): yes | no

## Acceptance Criteria

- [ ] <observable acceptance criterion>

## Business Rules / References

<!-- Link relevant docs/user_stories/ items and docs/REGRAS_NEGOCIO_COMPLETO.md sections when business rules change -->

- <reference or n/a>

## Edge Cases

- <empty, null, invalid input, permission denied, network/Supabase errors, etc.>

## E2E Baseline Policy

<!--
  Decide once here. TDD/Code/Review enforce it.

  Blocking when ANY of these is true:
  - Critical flow = yes
  - Frontend change = yes AND Business-rule change = yes
  - Refactor of pre-existing hotspot = yes

  Recommended when:
  - Touches pre-existing code = yes (and not blocking by the above)

  Not applicable when:
  - Greenfield code with no pre-existing flow at risk
-->

- Decision: blocking | recommended | not applicable
- Rationale: <why>
- If blocking and no E2E exists: the first Code task group is to create the baseline E2E for the affected flow before any production code change.

## Manual Validation

<!-- For user-visible features, decide if docs/testes/<FEATURE>.md is required under roteiros-de-teste.mdc -->

- Roteiro manual required: yes | no | tbd
- Business analyst validation environment: homologacao | producao | n/a

## Release / Judge Gate

- [ ] Product owner accepts the PRD scope.
- [ ] Open questions below are resolved or explicitly deferred.
- [ ] Acceptance criteria are specific enough to drive Specs, BDD, and TDD.
- [ ] Business-rule references and manual-validation needs are documented.
- [ ] Change classification and E2E baseline policy are recorded.

## Open Questions

- <question>
