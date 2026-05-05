# Product Requirements Document

> Stakeholder Requirements Specification — ISO/IEC/IEEE 29148:2018

## Problem

<!-- What problem are we solving? Why now? -->

## Goals

<!-- User/business outcomes that must be achieved -->

## Non-Goals

<!-- Explicitly out of scope -->

## Users / Actors

<!-- Personas, roles, permissions, or systems involved -->

## Scope

<!-- In-scope product behavior, routes, modules, data, and integrations -->

## Capabilities

### New Capabilities

- `<capability-name>`: <brief description>

### Modified Capabilities

- `<existing-capability>`: <requirement-level behavior change>

## Change Classification

<!--
  This drives the E2E baseline policy below and the gates in TDD/Code/Review.
  Be honest: false negatives here cause production regressions.
-->

- Touches pre-existing code: yes | no
- Frontend (UI/UX) change: yes | no
- Business-rule change: yes | no  (refer to docs/REGRAS_NEGOCIO_COMPLETO.md and docs/user_stories/)
- Critical flow per docs/qualidade/ESTRATEGIA_ESTEIRA_QA.md §2: yes | no
- Refactor of pre-existing hotspot (codopsy grade D/E/F): yes | no

## Acceptance Criteria

- [ ] <observable acceptance criterion>

## Business Rules / References

<!-- Link relevant docs/user_stories/ items and docs/REGRAS_NEGOCIO_COMPLETO.md sections when business rules change -->

- <reference or n/a>

## Edge Cases

- <empty, null, invalid input, permission denied, network/Supabase errors, etc.>

## E2E Baseline Policy

<!--
  Decide once here. TDD/Code/Review enforce it.

  Blocking when ANY of these is true:
  - Critical flow = yes
  - Frontend change = yes AND Business-rule change = yes
  - Refactor of pre-existing hotspot = yes

  Recommended when:
  - Touches pre-existing code = yes (and not blocking by the above)

  Not applicable when:
  - Greenfield code with no pre-existing flow at risk
-->

- Decision: blocking | recommended | not applicable
- Rationale: <why>
- If blocking and no E2E exists: the first Code task group is to create the baseline E2E for the affected flow before any production code change.

## Manual Validation

<!-- For user-visible features, decide if docs/testes/<FEATURE>.md is required under roteiros-de-teste.mdc -->

- Roteiro manual required: yes | no | tbd
- Business analyst validation environment: homologacao | producao | n/a

## Release / Judge Gate

- [ ] Product owner accepts the PRD scope.
- [ ] Open questions below are resolved or explicitly deferred.
- [ ] Acceptance criteria are specific enough to drive Specs, BDD, and TDD.
- [ ] Business-rule references and manual-validation needs are documented.
- [ ] Change classification and E2E baseline policy are recorded.

## Open Questions

- <question>
