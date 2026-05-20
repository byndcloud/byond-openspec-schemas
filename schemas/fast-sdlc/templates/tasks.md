# Tasks

> Implementation checklist. Tests come first. Order matters: each group depends on the previous.

## 1. Tests First

- [ ] 1.1 Create or update the failing tests listed in design.md > Test Plan.
- [ ] 1.2 Run the focused gate command(s) and confirm tests fail for the expected reason.

## 2. Implementation

- [ ] 2.1 Implement the smallest production change that turns the tests green.
- [ ] 2.2 Keep the change scoped to the PRD/Specs contract — no extra refactors.
- [ ] 2.3 Update generated types or run migrations if Design called for it.

## 3. Verification

- [ ] 3.1 Focused unit / integration tests pass.
- [ ] 3.2 Lint and type checks pass.
- [ ] 3.3 E2E flow (if applicable) passes.
- [ ] 3.4 `npx openspec validate <change-name>` passes.

## 4. Review Handoff

- [ ] 4.1 Fill in review.md with the gate result and final decision.
- [ ] 4.2 No unchecked tasks remain above this line before archiving.
