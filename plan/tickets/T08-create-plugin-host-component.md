# T08 — Create plugin host component

## Summary
Build the common runtime shell that renders plugin apps.

## Goals
- Keep plugins visually consistent
- Inject shared services in one place

## Scope
- Create plugin host layout
- Provide loading/error/empty states
- Inject progress and event services
- Standardize plugin header/footer controls

## Deliverables
- Shared plugin host
- Error-safe rendering boundary

## Acceptance Criteria
- Plugins render through the host
- Shared controls appear consistently
- Runtime errors do not crash the whole app
