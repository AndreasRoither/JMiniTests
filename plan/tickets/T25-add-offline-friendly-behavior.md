# T25 — Add offline-friendly behavior

## Summary
Improve study reliability when connectivity is limited.

## Goals
- Keep core content usable offline where possible
- Handle partial offline states gracefully

## Scope
- Cache lesson content
- Cache core plugin assets
- Offline messaging
- Re-sync on reconnect if needed

## Deliverables
- Offline-aware content behavior

## Acceptance Criteria
- Previously loaded lessons remain readable offline
- Offline state is communicated clearly
- App does not fail silently when interactive content is unavailable
