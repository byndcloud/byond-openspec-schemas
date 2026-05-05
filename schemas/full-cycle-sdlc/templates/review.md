# Verification & Validation Report (Review Gate)

> IEEE 1012-2016

## Judge Decision

Status: pass | conditional-pass | fail

Reviewer/Judge: <name or role>

## Evidence Reviewed

- PRD: `prd.md`
- Specs delta: `specs/<capability>/spec.md`
- SDD: `sdd.md`
- BDD: `bdd.md`
- TDD: `tdd.md`
- Code tasks: `tasks.md`
- Mutation gate: `mutation.md`

## Baseline E2E Gate

- PRD baseline policy: blocking | recommended | not applicable
- Baseline confirmed/created before code change: yes | no | n/a
- Evidence: <path to E2E test, run timestamp, or commit hash>
- Exception (only when policy != blocking): <reason or n/a>

## Acceptance Criteria Review

| PRD Criterion | Evidence               | Status        |
| ------------- | ---------------------- | ------------- |
| <criterion>   | <test/manual evidence> | pass/fail/n-a |

## Spec Sync Readiness

- [ ] Delta requirements at `specs/<capability>/spec.md` are complete and ready to be archived into `openspec/specs/<capability>/spec.md`.
- [ ] MODIFIED requirements include the full updated block (not partial).
- [ ] REMOVED requirements include Reason and Migration.

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

- [ ] `code-review.mdc`: self-review completed before declaring ready.
- [ ] `testing-and-ci.mdc`: relevant test pyramid gates executed or blockers documented.
- [ ] `frontend-patterns.mdc`: React Query, RHF+zod, env, `data-testid`, sanitization, and hour-calculation rules satisfied where applicable.
- [ ] `database-workflow.mdc`: migrations, RLS, pgTAP, and generated types satisfied where applicable.
- [ ] `database-functions.mdc`: RPC DROP + CREATE OR REPLACE, `search_path`, grants, and pgTAP satisfied where applicable.
- [ ] `edge-functions.mdc`: Deno env, CORS, auth/service role, contract, and tests satisfied where applicable.
- [ ] `accessibility.mdc`: WCAG 2.1 AA expectations checked for new UI/refactors.
- [ ] `roteiros-de-teste.mdc`: manual business script created or explicitly not required.

## Blockers

- <blocker or none>

## Follow-ups

- <non-blocking follow-up or none>

## Final Gate

- [ ] PRD acceptance criteria are satisfied or explicitly deferred.
- [ ] Specs delta is ready to archive (canonical sync will be triggered by `openspec archive`).
- [ ] SDD decisions were followed or deviations are documented.
- [ ] BDD/TDD evidence supports the implementation.
- [ ] When baseline policy = blocking, baseline E2E was confirmed or created before code changes.
- [ ] Mutation gate is pass or accepted with reviewed exceptions.
- [ ] No critical review findings remain unresolved.
- [ ] Any skipped validation is explained as infra/pre-existing/unrelated, not silently ignored.
