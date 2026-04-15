# T10 — Build progress tracking service

## Summary
Create the shared service for tracking lesson and plugin progress.

## Goals
- Persist learner state consistently
- Support summary views later

## Scope
- Completed lessons
- Last active item
- In-progress state
- Mastery placeholders
- Saved/bookmarked integration points

## Deliverables
- Progress service API
- Persistence hooks

## Acceptance Criteria
- Lessons can be marked complete
- In-progress state persists
- Progress can be read by dashboard and plugins
