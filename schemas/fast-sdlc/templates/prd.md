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
