# Tasks

> Implementation checklist. Tests come first. Order matters: each group depends on the previous.

## 0. Baseline E2E (only when PRD E2E Baseline Policy = blocking)

<!-- Skip this whole group only when the policy is "recommended" or "not applicable". -->

- [ ] 0.1 Confirm or create the baseline E2E (Playwright `e2e/<flow>.pw.ts`) for the affected flow.
- [ ] 0.2 Run it green against UNCHANGED behavior before any production code change.

## 1. Tests First (mandatory — no production code before this group is green-checked)

- [ ] 1.1 Create or update the failing tests for every row in design.md > Test Plan. Tests come BEFORE any production code in group 2.
- [ ] 1.2 Run the focused gate command(s) and confirm each test fails for the expected reason (the RED state).
- [ ] 1.3 Record the RED evidence: paste the failing command output (or the assertion message) so the test-first order is auditable in the PR / review.

## 2. Implementation (only after group 1 shows RED)

- [ ] 2.1 Implement the smallest production change that turns the tests green.
- [ ] 2.2 Keep the change scoped to the PRD/Specs contract — no extra refactors.
- [ ] 2.3 Update generated types or run migrations if Design called for it.

## 3. Verification

- [ ] 3.1 Focused unit / integration tests pass.
- [ ] 3.2 Lint and type checks pass.
- [ ] 3.3 Baseline E2E passes — REQUIRED when E2E Baseline Policy = blocking; "if applicable" only for recommended / not-applicable.
- [ ] 3.4 `npx openspec validate <change-name>` passes.

## 4. Review Handoff

- [ ] 4.1 Fill in review.md with the gate result and final decision.
- [ ] 4.2 No unchecked tasks remain above this line before archiving.
