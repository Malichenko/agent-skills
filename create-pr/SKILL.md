---
name: create-pr
description: >
  Creates a Pull Request from currently staged git files. Groups staged files into
  domain zones, makes a separate commit per zone, runs an AI code review, and opens a
  PR with a generated description. Blocks on critical (🔴) issues before committing.
  Use when user says "create PR", "open PR", "make PR", "ship this", or invokes /create-pr.
  IMPORTANT: operates only on staged files (git diff --cached). Never touches unstaged changes.
---

# Create PR from Staged Files

## Workflow

### Step 1 — Guard: staged files only

Run:
```
git diff --cached --name-only
```

If the output is empty → **stop immediately**:
> No staged files found. Stage your changes with `git add` first, then run `/create-pr` again.

**Save the staged file list** — this is the exact set of files to work with. Nothing else.

Then immediately run:
```
git reset HEAD
```

This unstages all files. This is required so that per-zone `git add` in Step 6 stages only the zone's files and nothing else. Without this, a `git commit` would include all previously staged files regardless of what you `git add`.

Never touch unstaged files. Never run `git add .` or `git add -A`.

---

### Step 2 — Collect the full diff

Run:
```
git diff --cached
```

Use this diff for both grouping (Step 3) and the AI Check (Step 4).

---

### Step 3 — Group files into domain zones

Analyze the staged file list and assign each file to exactly one domain zone using these rules (first match wins):

| Zone label | Match pattern |
|---|---|
| `deps` | `package.json`, `package-lock.json`, `*.lock`, `mise.toml` |
| `config` | `*.config.*`, `tsconfig*`, `next.config*`, `vitest*`, `.env*`, `vercel.json` |
| `app/<segment>` | `app/**` — group by top-level app segment |
| `feature/<name>` | `src/features/<name>/**` — group by feature name |
| `components` | `src/components/**` |
| `containers` | `src/containers/**`, `src/pageContainers/**` |
| `store` | `src/store/**` |
| `hooks` | `src/hooks/**` |
| `utils` | `src/utils/**`, `src/services/**`, `src/helpers/**` |
| `styles` | `**/*.css`, `**/*.scss`, `src/styles/**`, `src/theme/**` |
| `tests` | `**/*.test.*`, `**/*.spec.*`, `**/__tests__/**` |
| `types` | `**/*.types.*`, `src/types/**` |
| `misc` | everything else |

Rules:
- A single logical change spanning a feature dir + its tests → keep in one zone, label by feature.
- Lockfile always goes alone in `deps` unless the only other file is `package.json` (then merge).
- If only one file total → one commit covering it.

---

### Step 4 — AI Check (BLOCKING)

Review the full diff from Step 2. Apply the same rules as `/gen-pr-desc`:

Severity levels:
- 🔴 **bug** — broken behavior, will cause a runtime error or data loss
- 🟡 **risk** — fragile, unsafe cast, missing null guard at a boundary, broken contract
- 🔵 **nit** — style, naming, redundant expression, dead code

**If any 🔴 bugs are found → STOP. Do not commit. Do not push.**

Output:
```
🚨 Critical issues found — fix these before the PR is created:

🔴 <file>:<line> — <problem>. <fix>.
...

Fix the issues above, re-stage the files, and run /create-pr again.
```

If only 🟡/🔵 findings → note them but continue. They will appear in the PR description.

If no issues → continue.

---

### Step 5 — Show the plan and ask for approval

Present the grouping clearly:

```
Staged files grouped into N commit(s):

1. chore(deps): bump features-icons 2.0.24 → 2.1.0, random-rewards 0.12.10 → 0.12.14
   • package.json
   • package-lock.json

2. feat(chain-offers): add isGridLayout prop support
   • src/features/features-icons/.../ChainOffersConfig.tsx

3. refactor(random-rewards): replace explicit props with options passthrough
   • src/features/features-icons/.../RandomRewardsConfig.tsx
   • src/features/random-rewards/.../RandomRewardsHomePageIcon/index.tsx

Proceed? (yes / adjust)
```

Wait for explicit approval. If the user wants to adjust labels or grouping, apply their changes before continuing.

---

### Step 6 — Commit each zone

Before committing, extract any Jira ticket ID from the current branch name. A Jira ticket ID matches the pattern `[A-Z]+-[0-9]+` (e.g. `REENG-3652`, `ACC-6034`). If found, prepend it to every commit message:

```
REENG-3652 feat(chain-offers): add isGridLayout prop support
```

If no ticket ID is found in the branch name, omit the prefix entirely.

For each zone, in order:

```bash
git add <files-in-zone...>
git commit -m "<[TICKET-ID ]conventional-commit-message>"
```

Commit message format: `[TICKET-ID ]<type>(<scope>): <short description>`
Types: `feat`, `fix`, `refactor`, `chore`, `style`, `test`, `docs`

Never add `Co-Authored-By` trailers or any AI attribution to commit messages.

After all commits, verify with `git log --oneline -<N>`.

---

### Step 7 — Generate PR description

Using the diff and the AI Check findings from Steps 2 and 4, build the PR body:

```markdown
## What was done

- <bullet per zone — what changed and why>

## AI Check

<findings from Step 4, or "No issues detected.">
```

---

### Step 8 — Ask for target branch

Before pushing, always ask:

> Which branch should this PR target? (e.g. `master-mcluck`, `main`, `develop`)

Wait for the user's answer. Do not assume or infer the base branch.

---

### Step 9 — Push and open PR

```bash
git push -u origin <current-branch>
gh pr create \
  --base <user-confirmed-target-branch> \
  --title "<derived from branch name + changes>" \
  --body "<PR body from Step 7>"
```

Return the PR URL to the user.

---

## Boundaries

- **Never** stage unstaged files.
- **Never** use `--no-verify` or bypass Husky hooks.
- **Never** force-push.
- **Never** proceed past Step 4 if a 🔴 bug is found.
- If `gh` is not installed or not authenticated, output the PR body and instruct the user to open the PR manually.