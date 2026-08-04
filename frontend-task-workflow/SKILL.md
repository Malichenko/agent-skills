---
name: frontend-task-workflow
description: Follow a requirement-to-verification workflow for React, Next.js, and TypeScript frontend work. Use when implementing features, fixing UI bugs, refactoring, testing, reviewing, or preparing PR-ready frontend changes.
---

# Frontend Task Workflow

Use this skill when working on a React / Next.js / TypeScript frontend task, especially when the task comes from Jira, Linear, GitHub issue, or product requirement.

## Goal

Move from requirement to implementation safely:

requirement → codebase research → plan → implementation → tests → review → verification → documentation.

## When to use

Use this skill for:

- implementing frontend features
- fixing UI bugs
- refactoring frontend code
- adding or changing React components
- changing Next.js pages or routes
- changing data fetching logic
- implementing Jira or GitHub issue requirements
- preparing PR-ready changes

## Workflow

### 1. Understand the requirement

Read the task carefully and extract:

- user-visible behavior
- acceptance criteria
- out-of-scope items
- unclear requirements
- affected pages, components, APIs, or user flows

If the task comes from Jira or GitHub, treat the ticket as the source of truth.

### 2. Inspect the codebase

Before editing files:

- apply frontend-search-first
- inspect existing components
- inspect existing hooks
- inspect existing types
- inspect existing tests
- inspect existing styling conventions
- inspect package.json scripts
- identify server/client component boundaries if using Next.js

Do not implement from assumptions when the project already has a pattern.

### 3. Plan

Create a short plan with:

- current pattern
- proposed solution
- affected files
- edge cases
- test plan
- risks and assumptions

Ask for approval before editing unless the user explicitly asked to implement immediately.

### 4. Implement

When approved:

- make small changes
- reuse existing components, hooks, utilities, and types
- keep public APIs stable unless required
- avoid new dependencies unless approved
- keep UI states explicit: loading, error, empty, disabled
- preserve accessibility and responsive behavior

### 5. Test

For UI changes, consider:

- rendering required content
- optional or missing data
- loading state
- error state
- empty state
- disabled state
- keyboard interaction
- accessible names, labels, and alt text
- long text and layout edge cases
- user interactions

Use React Testing Library where applicable.

### 6. Review

Review the diff for:

- correctness
- type safety
- accessibility
- performance
- maintainability
- edge cases
- consistency with project patterns
- test quality

### 7. Verify

Run or suggest available scripts from package.json:

- format check
- typecheck
- lint
- tests
- build

Do not invent scripts. Inspect package.json first.

### 8. Document

If the change affects reusable components, architecture, onboarding, or public usage:

- update existing docs
- update Storybook docs if used
- document actual behavior only
- include examples only when they match the codebase

## Output style

Be concise and practical.
Prefer structured answers.
Separate blockers from suggestions.
Do not hide uncertainty.
