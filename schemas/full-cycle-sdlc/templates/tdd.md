## Test-First Principle

Tests in this plan MUST be derived from `prd.md`, `sdd.md`, and `bdd.md`.
Do not use the existing implementation as the expected-behavior source of truth.

## Test Matrix


| Layer    | Test Target                      | Source Artifact         | Expected Initial State                    | Gate Command               |
| -------- | -------------------------------- | ----------------------- | ----------------------------------------- | -------------------------- |
| Unit     | <helper/hook/component behavior> | <PRD/SDD/BDD reference> | Fails before code                         | `npx vitest run <file>`    |
| DB pgTAP | <migration/RPC/RLS behavior>     |                         | Fails before code                         | `npm run test:db`          |
| Edge     |                                  | <SDD/BDD reference>     | Fails before code or exception documented | <Deno/Vitest/HTTP command> |
| E2E      |                                  |                         | Fails before code                         | `npm run test:e2e`         |
| Manual   |                                  | <PRD/BDD reference>     | Roteiro drafted before release            | <docs/testes path>         |


## Unit Tests

- 

## Integration / Database Tests

- 

## Edge Function Tests

- 

## E2E / BDD Automation

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

- Test plan covers PRD acceptance criteria and BDD scenarios.
- Required failing tests are identified before implementation.
- Test data, mocks, and environment assumptions are explicit.
- Gate commands are listed and runnable or blockers are documented.
- Schema/RPC/RLS changes include pgTAP; Edge changes include tests or documented exception.
- Critical frontend flows prefer semantic Testing Library queries and define `data-testid` needs only where stable selectors are required.

## Open Questions

- 