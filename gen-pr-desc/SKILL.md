---
name: gen-pr-desc
description: >
  Generates a structured Pull Request description from the current git diff. Produces
  two sections: "What was done" (concise list of key changes) and "AI Check" (code review
  against best practices, potential bugs, and style issues). Use when user says "generate
  PR description", "write PR desc", "make a PR description", "@diff", or invokes /gen-pr-desc.
---

# Generate PR Description

## Workflow

1. Run `git diff HEAD` to capture all staged and unstaged changes vs the last commit.
2. If the diff is empty, run `git diff origin/HEAD..HEAD` to diff against the remote base branch.
3. Analyze the diff and produce the PR description below.

## Output format

Use this exact structure (markdown, no extra headers):

```
## What was done

- <concise bullet per logical change — what changed and why, not just file names>
- ...

## AI Check

<One finding per line: `file:line — problem. fix.` Use severity prefix when mixing:>
- 🔴 bug: broken behavior
- 🟡 risk: fragile, edge-case, type-safety
- 🔵 nit: style, naming, cleanup

If no issues found, write: **No issues detected.**
```

## Rules

**What was done**
- Group related file changes into one bullet (e.g. "bumped X from 1.0 → 2.0 + updated callsites").
- Lead with the *why* when it's known from context (ticket, branch name, config change).
- Skip lockfile-only entries unless the version bump is the point of the PR.

**AI Check**
- Flag: unused declared props/vars, `any` casts without justification, redundant expressions, missing null guards at boundaries, broken public API contracts.
- Don't flag: intentional patterns consistent with the existing codebase, lockfile changes, generated files.
- Include file path and line number when pointing to a specific location.
- If the project has a CLAUDE.md, apply its rules (Tailwind arbitrary values, `middleware.ts`, package manager, etc.) as additional check criteria.

## Boundaries

- Does not create or push the PR — output only.
- Does not modify any files.
- If the diff is very large (>500 changed lines), summarize by area rather than listing every file.