# Japanese Learning Platform — Implementation Plan

## Overview

This project is a Japanese learning platform built with **Astro** as the host application and **React** for interactive learning modules. The platform should support **PC, tablet, and phone**, render lessons from **Markdown/MDX**, and host several smaller learning applications through an internal **plugin-style architecture**.

The platform should support:

- learning content by **JLPT level** from **N5 to N1**
- structured study content such as lessons, grammar notes, vocabulary, and exercises
- interactive practice modules for topics like time, days, months, dates, and numbers
- future testing modes such as quizzes, timed tests, and mock exams
- progress tracking, review support, bookmarks, search, and filtering
- a future extension point for **drawing hiragana, katakana, and kanji**

## Goals

- Build a content-first Japanese learning app that stays fast and responsive
- Create a host application that can load many smaller interactive learning modules
- Keep lessons easy to write and maintain using Markdown and MDX
- Support a long-term content model that works from beginner through advanced JLPT levels
- Track study progress in a way that is meaningful for language learning
- Prepare for future assessment and handwriting practice without overbuilding v1

---

## Product structure

The platform should be split into five clear domains:

1. **Content**
   - lessons
   - vocabulary
   - grammar
   - exercises

2. **Practice**
   - plugins
   - drills
   - quizzes
   - interactive reference tools

3. **Assessment**
   - test mode
   - timed quizzes
   - mock exams
   - score history

4. **Progress**
   - completion
   - study time
   - mastery
   - weak items
   - review queue
   - saved items

5. **Discovery**
   - search
   - filtering
   - bookmarks
   - browse by level/topic/type

---

## Recommended stack

### Core stack

- **Astro** for routing, layouts, content delivery, and static-first rendering
- **React** for interactive learning modules
- **TypeScript** for shared models and safer plugin/content contracts
- **Tailwind CSS** for responsive UI and fast iteration
- **Markdown** for most lessons
- **MDX** for lessons that need embedded interactive widgets

### Data and persistence

- local-first persistence for v1
- `localStorage` for simple user settings and lightweight saved state
- `IndexedDB` for larger progress/event history if needed
- versioned storage schema from the start

### Testing

- **Vitest** for logic and services
- **React Testing Library** for interactive components
- **Playwright** for mobile/tablet/desktop end-to-end flows

---

## Architecture

## 1. Host application

Astro should own:

- routing
- layouts
- navigation
- dashboard pages
- lesson pages
- content indexing
- static generation where possible

React should own:

- quizzes
- drills
- timers
- interactive cards
- plugin host UI
- assessment UI
- future handwriting widgets

## 2. Plugin architecture

The smaller applications should be implemented as **internal plugins/modules**, not external packages in v1.

Each plugin should have:

- a manifest
- a unique ID
- level and topic metadata
- capability flags
- route information
- one React entry component
- access to shared services for progress, tracking, and review

### Example plugin contract

```ts
export type SupportedLevel = "N5" | "N4" | "N3" | "N2" | "N1";

export type LearningPlugin = {
  id: string;
  title: string;
  description: string;
  version: string;
  category: "vocab" | "grammar" | "reading" | "quiz" | "writing" | "time" | "test";
  levels: SupportedLevel[];
  tags: string[];
  searchable: boolean;
  lessonRefs?: string[];
  capabilities: {
    tracksRuntime: boolean;
    supportsReview: boolean;
    supportsTestMode: boolean;
    supportsMockExamMode?: boolean;
    supportsDrawing?: boolean;
  };
  mount: React.ComponentType<PluginProps>;
};
```

## 3. Shared services

Shared services should be created in the core app rather than inside plugins:

- plugin registry
- progress service
- runtime/event tracking service
- review queue service
- search indexing service
- content lookup service
- answer normalization service
- storage service

## 4. Content model

Use content collections for:

- lessons
- vocabulary
- grammar
- exercises
- tests
- reference data

Each content item should support:

- title
- slug
- level or levels
- topic
- tags
- estimated time
- related items
- plugin references if needed

### Example lesson frontmatter

```md
---
title: "Days of the Week"
slug: "days-of-week"
levels: ["N5"]
topic: "time"
tags: ["calendar", "time", "vocabulary"]
estimatedMinutes: 10
relatedPlugins: ["days"]
---
```

---

## Information architecture

The platform should support these major user views:

- Home / Dashboard
- Lessons
- Practice
- Tests
- Saved
- Review
- Search
- Plugin detail pages
- Level pages such as N5, N4, N3, N2, N1

### Browse paths

Users should be able to browse by:

- JLPT level
- topic
- content type
- learning state
- saved status

---

## JLPT level system

The level system should be built in from the start.

### Supported levels

- N5
- N4
- N3
- N2
- N1

### Requirements

- every lesson, exercise, plugin, and test should support one or more levels
- mixed-level content should be allowed
- dashboard summaries should show progress by level
- search and filtering should include level selection
- tests should support filtering by level and topic

---

## Search and filtering

Search should work across:

- lessons
- grammar notes
- vocabulary
- plugins
- tests
- exercises
- saved items

Filters should support:

- level
- topic
- type
- status
- saved/bookmarked
- due for review
- script type where relevant

### Example searchable item model

```ts
type SearchableItem = {
  id: string;
  kind: "lesson" | "plugin" | "exercise" | "vocab" | "grammar" | "test";
  title: string;
  description?: string;
  levels: SupportedLevel[];
  tags: string[];
  topic?: string;
  status?: "not_started" | "in_progress" | "completed";
};
```

---

## Progress and runtime tracking

The app should track learning progress in a way that is helpful for study, not just analytics.

### Track:

- lesson completion
- quiz attempts
- test attempts
- saved items
- bookmarked items
- last active study area
- study time by plugin
- study time by topic
- study time by level
- weak items
- due review items

### Event model

Use an event-based model for tracking, then derive summaries from it.

```ts
type LearningEvent = {
  id: string;
  pluginId?: string;
  contentId?: string;
  type:
    | "session_started"
    | "session_ended"
    | "lesson_opened"
    | "lesson_completed"
    | "question_answered"
    | "hint_used"
    | "review_scheduled"
    | "test_started"
    | "test_finished"
    | "item_saved";
  timestamp: string;
  durationMs?: number;
  metadata?: Record<string, unknown>;
};
```

### Progress model

```ts
type UserProgress = {
  completedLessonIds: string[];
  bookmarkedIds: string[];
  savedItemIds: string[];
  masteryByTopic: Record<string, number>;
  masteryByLevel: Record<SupportedLevel, number>;
  quizAttempts: QuizAttempt[];
  testAttempts: TestAttempt[];
  reviewQueue: ReviewItem[];
};
```

---

## Assessment system

Testing should be treated as a product feature, not just a plugin detail.

### Modes

- Learn mode
- Practice mode
- Review mode
- Test mode
- Mock exam mode

### Future testing support

The system should allow:

- topic-specific tests
- level-specific tests
- mixed review tests
- timed tests
- saved test history
- weak-item follow-up after tests

### Test content model

```ts
type TestDefinition = {
  id: string;
  title: string;
  levels: SupportedLevel[];
  topics: string[];
  mode: "practice_test" | "mock_exam" | "speed_quiz";
  itemRefs: string[];
  timed?: boolean;
  durationMinutes?: number;
};
```

---

## Responsive design

The app must work well on:

- phone
- tablet
- desktop

### Design rules

- mobile-first layouts
- no hover-only interactions
- touch-friendly controls
- tables must adapt to narrow screens
- study flows should work with both touch and keyboard

### Layout direction

- **phone**: single-column, compact navigation, card-based content
- **tablet**: split content/practice layouts where useful
- **desktop**: side navigation, larger content and practice area

---

## Edge cases and design constraints

These should be explicitly handled in implementation:

### Content and routing
- plugin route collisions
- lesson references to missing plugins
- missing or duplicated IDs
- mixed-level content tagging
- deleted or renamed content breaking saved progress

### Language-learning edge cases
- multiple accepted Japanese answers
- hiragana-only vs kanji answers
- full-width vs half-width character normalization
- irregular date and counter readings
- multiple scripts for the same concept
- typed answers needing normalization rules

### Runtime and persistence
- user idle time inflating study metrics
- tab hidden/background sessions
- storage schema changes
- multiple tabs writing progress
- dashboard summaries going stale

### UI and device behavior
- vocab tables on small screens
- long English glosses on mobile
- touch and keyboard parity
- plugin failure isolation
- offline lesson access with partially interactive pages

---

## Future handwriting support

Handwriting should be planned as a future capability domain.

### Future support goals

- drawing hiragana
- drawing katakana
- drawing kanji
- stroke capture
- replay and feedback
- writing practice history

### v1 preparation

- include `supportsDrawing` in plugin capabilities
- define a placeholder writing plugin area
- keep the future writing engine isolated from quiz logic

---

## Suggested folder structure

```txt
src/
  components/
    ui/
    lesson/
    layout/
    analytics/
  content/
    lessons/
    vocab/
    grammar/
    exercises/
    tests/
  features/
    plugin-runtime/
    progress/
    review/
    metrics/
    search/
    assessment/
    writing/
  plugins/
    days/
    months/
    calendar-days/
    numbers-counting/
    time/
  pages/
    lessons/
    practice/
    tests/
    review/
    saved/
    dashboard/
  lib/
    content/
    storage/
    analytics/
    search/
    types/
```

---

## Roadmap

## Phase 1 — Foundation and first working slice

- project setup
- content collections
- plugin registry
- progress tracking
- search/filter foundation
- level taxonomy
- one or two complete learning plugins
- responsive shell
- bookmarks and saved items
- basic review queue

## Phase 2 — Expand learning loop

- more plugins
- better dashboards
- stronger review system
- richer filtering
- more lesson types
- assessment mode framework
- first saved test results

## Phase 3 — Testing and mock exams

- timed tests
- mixed-topic tests
- mock exam flow
- score history
- test-based weak item review

## Phase 4 — Writing practice

- drawing support
- kana writing
- kanji writing
- stroke capture
- writing progress

---

## Epics and ticket summary

### Epic 1 — Project foundation
T01–T03

### Epic 2 — Content system
T04–T06

### Epic 3 — Plugin runtime
T07–T09

### Epic 4 — Shared learning services
T10–T12

### Epic 5 — First plugin set
T13–T17

### Epic 6 — Shared UI components
T18–T20

### Epic 7 — Dashboard and learner view
T21–T22

### Epic 8 — Persistence and device polish
T23–T25

### Epic 9 — Testing and quality
T26–T28

### Epic 10 — Future-ready handwriting foundation
T29–T30

### Epic 11 — Leveling and taxonomy
T31–T32

### Epic 12 — Search and discovery
T33–T35

### Epic 13 — Assessment system
T36–T38

### Epic 14 — Progress expansion
T39–T40

---

## Notes for implementation

- Keep plugins internal in v1
- Keep content canonical and separate from plugin presentation logic
- Build answer normalization early
- Track events, then derive summaries
- Add build-time validation for content and plugin references
- Keep MDX use selective so lesson pages stay lightweight
- Design search, levels, and progress as core infrastructure, not later extras
