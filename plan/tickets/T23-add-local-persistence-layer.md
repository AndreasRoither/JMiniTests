# T23 — Add local persistence layer

## Summary
Create the storage abstraction for saving progress and events.

## Goals
- Make saved state stable and versioned
- Keep storage logic centralized

## Scope
- Storage adapter
- Schema versioning
- Read/write helpers
- Migration hooks
- Import/export placeholders

## Deliverables
- Storage layer API

## Acceptance Criteria
- Progress survives reload
- Storage schema has a version
- Migration path exists for future changes
