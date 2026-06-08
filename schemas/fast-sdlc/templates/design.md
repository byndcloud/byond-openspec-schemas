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
  The TDD half collapsed. Test-first is mandatory: every test listed here
  MUST be written and fail (RED) before the production code that satisfies it.
  This table is REQUIRED and must not be left empty — at least one Unit row
  for any change with logic. An empty Test Plan fails the Judge Gate below.
-->

| Layer | Target | Initial state | Gate command |
| ----- | ------ | ------------- | ------------ |
| Unit  | <helper / function — REQUIRED for any logic change> | Fails before code (RED) | `<command>` |
| Integration / DB | <if applicable> | Fails before code (RED) | `<command>` |
| E2E (baseline) | <flow — REQUIRED when PRD E2E Baseline Policy = blocking; `e2e/<flow>.pw.ts`> | Green before code (baseline) | `npm run test:e2e -- <flow>.pw.ts` |

## Risks / Trade-offs

- <risk and mitigation, or none>

## Rollout

<!-- Feature flag? Direct deploy? Backfill? Just normal PR? -->

- <plan>

## Judge Gate

- [ ] Design traces back to PRD scope and acceptance criteria.
- [ ] Data and contract changes are explicit.
- [ ] Test Plan is non-empty and every row has a gate command (an empty Test Plan is a fail for any change with logic).
- [ ] Test plan covers the main scenarios from above, and each test is written to fail before its production code (test-first).
- [ ] If the PRD E2E Baseline Policy is blocking, the Test Plan includes the baseline E2E row (flow + `e2e/<flow>.pw.ts` + gate command).
- [ ] No critical area requires escalating to full-cycle-sdlc (billing, RLS, auth, hot refactor).

## Open Questions

- <question or none>
