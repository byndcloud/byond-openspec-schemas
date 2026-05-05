# Test Plan (Test-First / TDD)

> Master Test Plan — ISO/IEC/IEEE 29119-3

## Test-First Principle

Tests in this plan MUST be derived from `prd.md`, `specs/<capability>/spec.md`,
`sdd.md`, and `bdd.md`. Do not use the existing implementation as the
expected-behavior source of truth.

## Baseline / Regression Safety Net



- PRD baseline policy: blocking | recommended | not applicable
- Existing E2E covering the flow: 
- Plan if missing:
  - Confirm existing E2E baseline runs green before any code change, OR
  - Create new baseline E2E first (mapped to a Code task in `tasks.md`), OR
  - Document exception with explicit reason (only when policy = recommended/not applicable).

## Test Matrix


| Layer    | Test Target                      | Source Artifact         | Expected Initial State                    | Gate Command               |
| -------- | -------------------------------- | ----------------------- | ----------------------------------------- | -------------------------- |
| Unit     | <helper/hook/component behavior> | <PRD/Specs/SDD/BDD ref> | Fails before code                         | `npx vitest run <file>`    |
| DB pgTAP | <migration/RPC/RLS behavior>     | <Specs/SDD contract>    | Fails before code                         | `npm run test:db`          |
| Edge     |                                  | <Specs/SDD/BDD ref>     | Fails before code or exception documented | <Deno/Vitest/HTTP command> |
| E2E      |                                  |                         | Baseline passes; new behavior fails first | `npm run test:e2e`         |
| Manual   |                                  | <PRD/BDD reference>     | Roteiro drafted before release            | <docs/testes path>         |


## Unit Tests

- 

## Integration / Database Tests

- 

## Edge Function Tests

- 

## E2E / BDD Automation

- <baseline E2E for unchanged behavior, when policy = blocking>
- 

## Accessibility / UX Tests

- <semantic query, keyboard/focus check, aria feedback assertion, or manual check>

## Mutation Targets

- <pure helper, branch-heavy function, validation logic, or boundary to mutate>

## Fixtures / Test Data

- <accounts, rows, factories, mocks, or seed assumptions>

## Commands

```bash
npm run lint
npx vitest run <focused test files>
npm run test:db
npm run test:e2e
npm run test:all
```

## Judge Gate Before Code

- Test plan covers PRD acceptance criteria, Specs delta, and BDD scenarios.
- Required failing tests are identified before implementation.
- Test data, mocks, and environment assumptions are explicit.
- Gate commands are listed and runnable or blockers are documented.
- Schema/RPC/RLS changes include pgTAP; Edge changes include tests or documented exception.
- Critical frontend flows prefer semantic Testing Library queries and define `data-testid` needs only where stable selectors are required.
- When PRD baseline policy = blocking, baseline E2E plan is concrete (existing path confirmed OR new baseline planned as first Code task).

## Open Questions

- 

