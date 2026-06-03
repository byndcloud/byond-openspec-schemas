# Product Requirements Document

> Lean PRD — fast-sdlc schema. For changes where full-cycle-sdlc is overkill but spec-driven is too thin.

## Problem

<!-- What problem are we solving? Why now? 2-3 sentences. -->

## Goals

<!-- User/business outcomes that must be achieved -->

## Non-Goals

<!-- Explicitly out of scope -->

## Users / Actors

<!-- Personas, roles, permissions, or systems involved -->

## Scope

<!-- In-scope behavior, routes, modules, data, and integrations -->

## Capabilities

### New Capabilities

- `<capability-name>`: <brief description>

### Modified Capabilities

- `<existing-capability>`: <requirement-level behavior change>

## Acceptance Criteria

- [ ] <observable acceptance criterion>

## Business Rules / References

- <reference or n/a>

## Edge Cases

- <empty, null, invalid input, permission denied, network errors, etc.>

## Change Classification

<!--
  Drives the E2E Baseline Policy below and the gate in Tasks/Review.
  Be honest: a false "no" here is how a critical-flow regression ships.
-->

- Touches pre-existing code: yes | no
- Frontend (UI/UX) change: yes | no
- Business-rule change: yes | no  (refer to your business-rule docs and user stories)
- Critical flow per your QA strategy: yes | no
- Refactor of pre-existing hotspot (high complexity grade): yes | no

## E2E Baseline Policy

<!--
  Decide once here. Tasks and Review enforce it.

  BLOCKING when ANY of these is true:
  - Critical flow = yes
  - Frontend change = yes AND Business-rule change = yes
  - Refactor of pre-existing hotspot = yes

  RECOMMENDED when:
  - Touches pre-existing code = yes (and not blocking by the above)

  NOT APPLICABLE when:
  - Greenfield code with no pre-existing flow at risk

  When blocking, this is NOT optional in fast-sdlc: the first task group
  must confirm or create the baseline E2E and run it green before any
  production code change. "n/a" is only valid for the not-applicable case.
-->

- Decision: blocking | recommended | not applicable
- Rationale: <why>
- Affected flow + baseline E2E path: `e2e/<flow>.pw.ts` | none yet (create it first)

## Should this escalate to full-cycle-sdlc?

<!--
  Check this honestly. If any of the below is true, stop using fast-sdlc
  and re-do this change under full-cycle-sdlc:

  - The change touches billing, auth, RLS, or migrations on user data.
  - The change is a critical-flow refactor.
  - A regression here would directly affect customers, finance, or compliance.

  fast-sdlc skips mutation testing, formal BDD scenarios, and the judge
  ceremony — that is the trade-off.
-->

- Escalation required: yes | no
- If yes, why: <reason>

## Open Questions

- <question or none>
