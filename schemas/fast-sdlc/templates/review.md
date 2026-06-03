# Review

> Lightweight gate. fast-sdlc intentionally skips mutation testing and the formal judge ceremony from full-cycle-sdlc.

## Decision

Status: pass | conditional pass | fail

Reviewer:

## Acceptance Criteria Check

| PRD Criterion | Evidence | Status |
| ------------- | -------- | ------ |
|               | <test, screenshot, or commit ref> | pass / fail / n-a |

## Quality Check

- [ ] Acceptance criteria from PRD are satisfied.
- [ ] Delta spec is ready to be archived into the canonical spec.
- [ ] Lint and type checks ran green.
- [ ] Unit / integration tests ran green.
- [ ] Baseline E2E ran green — REQUIRED when PRD E2E Baseline Policy = blocking. A blocking policy with "n/a" or no green E2E is a FAIL, not a pass.
- [ ] No new console errors or warnings introduced.

## Commands Run

```bash
npm run lint
<unit test command>
<e2e command — required when E2E Baseline Policy = blocking, e.g. npm run test:e2e -- <flow>.pw.ts>
npx openspec validate <change-name>
```

## Blockers

- <blocker or none>

## Follow-ups

- <non-blocking item or none>

## Did this change need full-cycle-sdlc instead?

<!--
  Honest retrospective. Mark yes if during implementation you discovered
  that the change actually deserved the full pipeline (mutation, formal
  BDD, judge gate). This information helps future planning.
-->

- yes | no
- If yes, what triggered the realization: <reason>
