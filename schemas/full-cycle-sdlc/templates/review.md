## Judge Decision

Status: pass | conditional-pass | fail

Reviewer/Judge: 

## Evidence Reviewed

- PRD: `prd.md`
- SDD: `sdd.md`
- BDD: `bdd.md`
- TDD: `tdd.md`
- Code tasks: `tasks.md`
- Mutation gate: `mutation.md`

## Acceptance Criteria Review


| PRD Criterion | Evidence               | Status        |
| ------------- | ---------------------- | ------------- |
|               | <test/manual evidence> | pass/fail/n-a |


## Quality Review

- Functional correctness:
- SOLID/DRY and maintainability:
- Security/data/RLS:
- Performance:
- Type safety/lint:
- Tests/CI:
- Accessibility/UX/i18n:
- Docs/manual roteiro:

## Commands Reviewed

```bash
npm run lint
npx vitest run <focused test files>
npm run test:db
npm run test:e2e
npm run test:all
<mutation command>
```

## Cursor Rules Checklist

- `code-review.mdc`: self-review completed before declaring ready.
- `testing-and-ci.mdc`: relevant test pyramid gates executed or blockers documented.
- `frontend-patterns.mdc`: React Query, RHF+zod, env, `data-testid`, sanitization, and hour-calculation rules satisfied where applicable.
- `database-workflow.mdc`: migrations, RLS, pgTAP, and generated types satisfied where applicable.
- `database-functions.mdc`: RPC DROP + CREATE OR REPLACE, `search_path`, grants, and pgTAP satisfied where applicable.
- `edge-functions.mdc`: Deno env, CORS, auth/service role, contract, and tests satisfied where applicable.
- `accessibility.mdc`: WCAG 2.1 AA expectations checked for new UI/refactors.
- `roteiros-de-teste.mdc`: manual business script created or explicitly not required.

## Blockers

- 

## Follow-ups

- 

## Final Gate

- PRD acceptance criteria are satisfied or explicitly deferred.
- SDD decisions were followed or deviations are documented.
- BDD/TDD evidence supports the implementation.
- Mutation gate is pass or accepted with reviewed exceptions.
- No critical review findings remain unresolved.
- Any skipped validation is explained as infra/pre-existing/unrelated, not silently ignored.

