---
name: frontend-search-first
description: Inspect an existing React, Next.js, or TypeScript codebase before implementing, refactoring, testing, reviewing, or debugging frontend code. Use for frontend tasks where repository conventions and similar implementations should guide the work.
---

# Frontend Search First

Use this skill before implementing, refactoring, testing, or reviewing React / Next.js / TypeScript frontend code.

## Goal

Do not implement from assumptions. First inspect the existing codebase, project conventions, scripts, and similar implementations.

## When to use

Use this skill for:

- adding a new React component
- changing an existing component
- adding tests
- refactoring frontend code
- changing Next.js pages, layouts, routes, or server/client components
- changing hooks or utilities
- changing data fetching logic
- working from Jira, Linear, GitHub issue, or product requirements
- debugging build, lint, typecheck, or test failures

## Search checklist

Before proposing or editing code, inspect relevant existing code.

### Project structure

Look for:

- package.json
- tsconfig.json
- next.config.js or next.config.ts
- app or pages directory
- src directory
- components directory
- shared UI directory
- hooks directory
- utils or lib directory
- api, graphql, or services directory
- tests directory
- Storybook configuration

### Existing patterns

Search for similar:

- components
- hooks
- utilities
- types
- tests
- Storybook stories
- loading states
- error states
- empty states
- disabled states
- form patterns
- data fetching patterns
- GraphQL or REST usage
- Tailwind or design-token usage

### Testing conventions

Inspect:

- existing test files
- test runner
- test setup
- custom render utilities
- mocks
- fixtures
- naming conventions
- React Testing Library usage
- Playwright or E2E conventions if present

### Scripts

Inspect package.json before running commands.

Identify available scripts for:

- dev
- build
- typecheck
- lint
- test
- test:unit
- test:e2e
- format
- storybook

Do not invent scripts.

### Documentation

Look for:

- README
- docs directory
- Storybook MDX
- architecture notes
- contribution guide
- component usage examples

## Output

After searching, report:

1. Relevant files found
2. Existing project pattern
3. Reusable code
4. Missing information
5. Recommended approach
6. Risks and assumptions

## Hard rules

- Do not create a new pattern if an existing project pattern solves the task.
- Do not introduce new dependencies without approval.
- Do not edit files before understanding the relevant pattern.
- Do not assume package scripts exist.
- Do not ignore existing tests or fixtures.
- If the project has no matching pattern, say so clearly.
