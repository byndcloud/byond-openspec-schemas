## 1. Tests First

- 1.1 Create or update failing tests from `tdd.md` before production code.
- 1.2 Run the focused test command and record the expected failure.
- 1.3 Add or update pgTAP / Edge / E2E tests required by `tdd.md`.

## 2. Implementation

- 2.1 Implement the smallest production change needed to satisfy the tests.
- 2.2 Keep changes scoped to the PRD/SDD contract.
- 2.3 Update generated types or migrations if required by the SDD.
- 2.4 Apply relevant Cursor rules for touched surfaces: frontend, database, RPC, Edge, a11y, testing.

## 3. Verification

- 3.1 Run focused unit/integration tests.
- 3.2 Run relevant E2E/BDD automation.
- 3.3 Run lint/type checks.
- 3.4 Run mutation testing for the targets listed in `tdd.md`.
- 3.5 Run `npm run db:types` and `npm run test:db` if schema/RPC/RLS changed.

## 4. Review Handoff

- 4.1 Update `mutation.md` with the mutation gate result.
- 4.2 Update `review.md` with the final judge decision.
- 4.3 Confirm no unchecked critical tasks remain before archive or PR readiness.
- 4.4 Document skipped gates, infra blockers, and known unrelated failures.