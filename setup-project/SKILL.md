---
name: setup-project
description: "Set up Claude Code for the current repository: root CLAUDE.md, project .claude/settings.json, safe permissions, and optional ignore guidance."
---

Set up Claude Code for the current repository.

## Goal

Create a practical project-level Claude Code setup based on the actual repository.

This setup may include:

- `PROJECT_ROOT/CLAUDE.md`
- `PROJECT_ROOT/.claude/settings.json`
- optional `PROJECT_ROOT/.claudeignore` proposal

Do not create generic config. Inspect the repository first.

## Target locations

Use:

- `PROJECT_ROOT/CLAUDE.md`
- `PROJECT_ROOT/.claude/settings.json`

Do not create:

- `PROJECT_ROOT/.claude/CLAUDE.md`

unless the user explicitly asks for that nonstandard location.

## Process

1. Find the repository root.
2. Inspect the repository structure.
3. Read `package.json` if present.
4. Detect the package manager from lockfiles and project files.
5. Identify real scripts from `package.json`.
6. Identify stack and conventions:
   - framework
   - language
   - test runner
   - lint/format tools
   - styling approach
   - data fetching approach
   - build/deploy hints
   - component structure
   - testing structure
7. Check whether these files exist:
   - `CLAUDE.md`
   - `.claude/settings.json`
   - `.claude/settings.local.json`
   - `.claudeignore`
   - `.gitignore`
8. Propose changes before writing.
9. Ask for approval before creating or updating files.

## CLAUDE.md requirements

Create or update root `CLAUDE.md` with concise project-specific instructions.

Include only sections that match the actual project:

- Project overview
- Tech stack
- Package manager and commands
- Directory structure
- Development workflow
- Testing workflow
- Component conventions
- Styling conventions
- Data fetching conventions
- Environment variables
- Accessibility expectations
- Security notes
- PR/checklist expectations
- Things Claude must not do

Rules:

- Do not invent scripts.
- Do not invent conventions.
- Do not include secrets.
- Keep it concise.
- Prefer stable project rules over temporary task notes.
- Avoid duplicating global `~/.claude/CLAUDE.md` unless the project overrides it.

## .claude/settings.json requirements

Create or update `.claude/settings.json` for project-level Claude Code settings.

Use it for:

- allowed safe commands
- denied dangerous commands
- denied sensitive file reads/edits
- project-specific permission defaults

Prefer minimal permissions.

Before writing, detect the package manager and scripts from `package.json`.

### Allowlist guidance

Only include commands that actually exist in the project.

Examples:

- `Bash(git status)`
- `Bash(git diff)`
- `Bash(git diff --stat)`
- `Bash(git diff --name-only)`
- `Bash(git log --oneline -20)`
- `Bash(ls)`
- `Bash(find . -maxdepth 3 -type f)`

For package scripts, only allow real scripts found in `package.json`, for example:

- `Bash(npm run lint)`
- `Bash(npm run typecheck)`
- `Bash(npm run test)`
- `Bash(npm run build)`
- `Bash(pnpm lint)`
- `Bash(pnpm typecheck)`
- `Bash(pnpm test)`
- `Bash(pnpm build)`
- `Bash(yarn lint)`
- `Bash(yarn typecheck)`
- `Bash(yarn test)`
- `Bash(yarn build)`

### Denylist guidance

Deny destructive or risky commands:

- `Bash(rm -rf *)`
- `Bash(rm -rf .*)`
- `Bash(sudo *)`
- `Bash(chmod -R 777 *)`
- `Bash(chown -R *)`
- `Bash(git push --force*)`
- `Bash(git reset --hard*)`
- `Bash(git clean -fd*)`
- `Bash(npm publish*)`
- `Bash(pnpm publish*)`
- `Bash(yarn publish*)`

Deny sensitive files:

- `Read(.env)`
- `Read(.env.*)`
- `Read(**/.env)`
- `Read(**/.env.*)`
- `Read(**/*secret*)`
- `Read(**/*token*)`
- `Read(**/*credential*)`
- `Edit(.env)`
- `Edit(.env.*)`
- `Edit(**/.env)`
- `Edit(**/.env.*)`
- `Write(.env)`
- `Write(.env.*)`
- `Write(**/.env)`
- `Write(**/.env.*)`

## .claude/memory guidance

Claude Code stores project-specific session memory (PRDs, plans, notes) in `PROJECT_ROOT/.claude/memory/`. This directory is for **local use only** — it is personal to each developer and must not be committed to version control.

When setting up a project:

1. Add `.claude/memory/` to the project's `.gitignore`.
2. Note this in `CLAUDE.md` under a `## Claude-specific files` section so future contributors know where local Claude files live and that they are gitignored.

Example `.gitignore` entry:

```
# Claude Code local memory (personal, not for sharing)
.claude/memory/
```

Example `CLAUDE.md` note:

```markdown
## Claude-specific files

Store all project-specific Claude files (memory, PRDs, plans, notes) locally under `.claude/memory/`. This directory is gitignored — do not commit it.
```

## .claudeignore guidance

If the project benefits from an ignore file, propose `.claudeignore` as an optimization for excluding noisy context, not as the primary security boundary.

Good candidates:

- `node_modules/`
- `.next/`
- `dist/`
- `build/`
- `coverage/`
- `.turbo/`
- `.vercel/`
- generated files
- large snapshots

Do not rely on `.claudeignore` to protect secrets. Use `.claude/settings.json` deny permissions for sensitive files.

## Output format

Before writing:

1. Project facts found
2. Existing Claude Code config status
3. Proposed `CLAUDE.md` outline
4. Proposed `.claude/settings.json`
5. Optional `.claudeignore` proposal
6. Risks or assumptions
7. Approval request before writing

After writing:

1. Files created or updated
2. Important settings added
3. Commands allowed
4. Files/commands denied
5. Suggested verification step
