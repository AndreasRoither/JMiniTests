# T09 — Add plugin route mapping

## Summary
Create dynamic routing for plugin pages.

## Goals
- Make plugins addressable by ID/slug
- Keep routing centralized

## Scope
- Add `/practice/[id]` or equivalent route
- Resolve plugin from registry
- Handle unknown IDs
- Add route-level metadata

## Deliverables
- Plugin route pages

## Acceptance Criteria
- Registered plugins resolve to routes
- Unknown plugins show a safe fallback
- Route collisions are prevented by validation
