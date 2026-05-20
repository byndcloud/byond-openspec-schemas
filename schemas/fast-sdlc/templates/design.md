# Design

> Combined design + scenarios + test plan. Replaces full-cycle-sdlc's separate SDD, BDD, and TDD artifacts for medium-risk changes.

## Context

<!-- 2-3 sentences: what is the change in technical terms? -->

## Architecture

<!--
  Component boundaries, data flow, contracts.
  Use a small ASCII diagram or a bullet list.
  Skip what is not changing.
-->

## Decisions

- <decision 1 and rationale>
- <decision 2 and rationale>

## Data / Contracts

<!--
  Tables, columns, RPCs, edge functions, or API contracts touched.
  If schema changes: list migration steps and whether RLS/policies change.
-->

- <change or n/a>

## Behavior Scenarios

<!--
  Given/When/Then for the main flows and the most relevant edge cases.
  Keep it focused, not exhaustive — this is the BDD half collapsed inline.
-->

### Scenario: <name>

- **Given**
- **When**
- **Then**

### Scenario: <error / edge case>

- **Given**
- **When**
- **Then**

## Test Plan

<!--
  Short bullet list. The TDD half collapsed.
  For each layer: what gets tested and the command to run.
  These tests MUST be written before the production code (test-first).
-->

| Layer | Target | Initial state | Gate command |
| ----- | ------ | ------------- | ------------ |
| Unit  | <helper / function> | Fails before code | `<command>` |
| Integration / DB | <if applicable> | Fails before code | `<command>` |
| E2E   | <if applicable> | Fails before code | `<command>` |

## Risks / Trade-offs

- <risk and mitigation, or none>

## Rollout

<!-- Feature flag? Direct deploy? Backfill? Just normal PR? -->

- <plan>

## Judge Gate

- [ ] Design traces back to PRD scope and acceptance criteria.
- [ ] Data and contract changes are explicit.
- [ ] Test plan covers the main scenarios from above.
- [ ] No critical area requires escalating to full-cycle-sdlc (billing, RLS, auth, hot refactor).

## Open Questions

- <question or none>
