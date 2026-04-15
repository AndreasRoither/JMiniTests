# T11 — Build runtime metrics service

## Summary
Create event-based runtime and study tracking.

## Goals
- Measure study time and interactions meaningfully
- Avoid overcounting idle time

## Scope
- Session start/end
- Question timing
- Idle timeout logic
- Hidden tab handling
- Event write API

## Deliverables
- Event model
- Metrics writer
- Session timer logic

## Acceptance Criteria
- Events are written consistently
- Hidden tabs pause active timing
- Idle time is not counted as active study time
