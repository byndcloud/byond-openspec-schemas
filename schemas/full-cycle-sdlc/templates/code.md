# Implementation Plan

> Work Breakdown Structure — PMBOK / ISO 21500

<!--
  Implementation checklist. Apply parses `- [ ]` items, so keep that format.
  When PRD/TDD declares E2E baseline as blocking, group 1 is mandatory and
  must run BEFORE any production code change.
-->

## 1. Baseline (mandatory when PRD/TDD says blocking)

- [ ] 1.1 Confirm or create the E2E baseline for the affected flow.
- [ ] 1.2 Run the baseline E2E and record the unchanged behavior passing.
- [ ] 1.3 Commit the baseline E2E (when newly created) before any production code change.
- [ ] 1.4 If policy is `recommended` or `not applicable`, mark this group as N/A with the documented justification.

## 2. Tests First

- [ ] 2.1 Create or update failing tests from `tdd.md` (unit, integration, pgTAP, Edge, new E2E for changed behavior).
- [ ] 2.2 Run the focused test commands and record the expected failure.

## 3. Implementation

- [ ] 3.1 Implement the smallest production change needed to satisfy the tests.
- [ ] 3.2 Keep changes scoped to the PRD acceptance criteria and Specs delta.
- [ ] 3.3 Update generated types or migrations if required by the SDD.
- [ ] 3.4 Apply relevant Cursor rules for touched surfaces: frontend, database, RPC, Edge, a11y, testing.

## 4. Verification

- [ ] 4.1 Run focused unit/integration tests.
- [ ] 4.2 Run baseline E2E (when applicable) and the new E2E for changed behavior.
- [ ] 4.3 Run lint/type checks.
- [ ] 4.4 Run mutation testing for the targets listed in `tdd.md`.
- [ ] 4.5 Run `npm run db:types` and `npm run test:db` if schema/RPC/RLS changed.

## 5. Review Handoff

- [ ] 5.1 Update `mutation.md` with the mutation gate result.
- [ ] 5.2 Update `review.md` with the final judge decision.
- [ ] 5.3 Confirm no unchecked critical tasks remain before archive or PR readiness.
- [ ] 5.4 Document skipped gates, infra blockers, and known unrelated failures.
