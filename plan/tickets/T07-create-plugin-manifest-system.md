# T07 — Create plugin manifest system

## Summary
Create the manifest model and registry for internal learning plugins.

## Goals
- Standardize plugin discovery and metadata
- Support browse and routing features

## Scope
- Define plugin manifest type
- Add registry loader
- Support categories, levels, tags, capability flags
- Validate unique IDs and route slugs

## Deliverables
- Plugin manifest type
- Registry module
- Validation rules

## Acceptance Criteria
- Plugin manifests load from one registry
- Duplicate IDs are rejected
- Plugin metadata is queryable
