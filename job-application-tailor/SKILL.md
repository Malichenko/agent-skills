---
name: job-application-tailor
description: End-to-end workflow for tailoring Oleksii Malichenko's job application to a specific vacancy. Use when the user asks to prepare, review, tailor, adapt, or create a CV and cover letter for a concrete job posting. Creates or uses a separate git branch per vacancy, starts from the current CV, applies frontend-focused job matching rules, writes a cover letter, compiles LaTeX PDFs, and updates/uses the job search profile and application tracker.
---

# Job Application Tailor

Use this skill for concrete vacancies where Oleksii wants a tailored CV and cover letter.

## Source Of Truth

In the CV repository, read these files when available:

- `JOB_COMMANDS.md` for predefined `cmd:` chat commands and exact workflow behavior.
- `JOB_SEARCH_PROFILE.md` for target roles, salary, language risk, filtering rules, and messaging.
- `JOB_APPLICATION_TRACKER.md` and the Google Sheet linked from `JOB_SEARCH_PROFILE.md` for deduplication and application status.
- Current CV sources, especially `Oleksii_Malichenko_CV.tex` and `Oleksii_Malichenko_CV_ATS.tex`, or the file the user names.
- `cover_letter.tex` when writing or adapting a cover letter.
- `.cursor/rules/FAANG-HR-LaTeX-CV-Auto-Adapter.mdc` as historical source guidance if present.

Do not treat old rejected or skipped roles as reusable targets unless the user explicitly asks.

## CMD Command Handling

Treat `cmd:` messages as predefined workflow triggers:

- `cmd: sync-shortlist`: read and summarize the Google Sheet `Shortlist` tab without side effects.
- `cmd: process-shortlist`: process all actionable `Shortlist` rows.
- `cmd: prepare-shortlist`: process only `Decision = Prepare package`.
- `cmd: apply-shortlist`: process only `Decision = Applied`.
- `cmd: skip-shortlist`: process only `Decision = Skip`.
- `cmd: clean-shortlist`: clean `Shortlist` only when explicitly requested.
- `cmd: applied <Company> / <Role>`: update `Applications` as `pending`.
- `cmd: rejected <Company>`: update `Applications` as `rejected`.
- `cmd: interview <Company>`: update `Applications` as `process` or `interview`.
- `cmd: prepare-package <Company> / <Role>`: prepare a dedicated branch, two tailored CV variants, cover letter, and PDFs for one vacancy.

If a command conflicts with surrounding text, ask one concise clarification before taking irreversible actions.

## Branch Workflow

1. Inspect the current git branch and worktree status.
2. If the user provided a concrete vacancy/company, create or switch to a dedicated branch using the `codex/` prefix, for example:
   - `codex/company-role-tailor`
   - `codex/ayettstudios-frontend`
3. Keep unrelated user changes intact.
4. Make only vacancy-specific CV and cover-letter edits on that branch.
5. Do not commit unless the user asks for a commit.

If the worktree has unrelated changes, keep working around them and mention the risk only if it affects the tailoring.

## Vacancy Evaluation Before Editing

Before tailoring, evaluate whether the role is worth an application using the profile rules:

- frontend-only or clearly frontend-heavy;
- React/Next.js/TypeScript relevance;
- backend/full-stack risk;
- German language risk;
- German contract/payroll/insurance risk;
- salary fit when visible.

If the role should be skipped, say so clearly and avoid unnecessary CV edits unless the user insists.

## CV Tailoring

Use the `faang-latex-cv-adapter` principles:

- preserve LaTeX structure and formatting;
- keep the current CV as the base;
- align Summary, skills, and experience bullets with the job description;
- emphasize real strengths: React, Next.js, TypeScript, frontend architecture, reusable UI/internal libraries, Core Web Vitals, testing, GraphQL/REST/WebSockets, dashboards, data-heavy UI, Vercel, monorepos, accessibility;
- avoid inventing facts or metrics.

For frontend roles, keep positioning frontend-heavy and avoid making the candidate look backend-first.

By default, prepare two CV variants from the `main` branch templates for each selected vacancy:

- `Oleksii_Malichenko_CV.tex` as the primary designed CV;
- `Oleksii_Malichenko_CV_ATS.tex` as the ATS-friendly CV.

Compile and report both generated CV PDFs together with the cover letter PDF.

## Cover Letter Workflow

Write or adapt `cover_letter.tex` for the vacancy:

- keep it concise, specific, and recruiter-friendly;
- mention company/role accurately;
- connect the candidate's strongest evidence to the role's needs;
- include English business fluent and German B1 only when useful;
- avoid generic enthusiasm without evidence;
- avoid overexplaining relocation/location unless needed.

Default salary expectation when needed:

- EUR 75,000 gross/year;
- EUR 80,000-85,000 for stronger senior roles or visible bands above EUR 80,000;
- avoid EUR 60,000 unless the role is attractive or lower-level.

## Build And Verify

After edits:

1. Compile the changed LaTeX files using the repository's normal workflow (`make`, Tectonic, or existing commands), including `Oleksii_Malichenko_CV.tex`, `Oleksii_Malichenko_CV_ATS.tex`, and `cover_letter.tex` by default.
2. Fix LaTeX syntax errors, obvious overflows, or broken PDFs caused by the edits.
3. Report the generated PDF filenames.

## Tracker Updates

When the user says they applied, update the Google Sheet tracker if available and sync the local `JOB_APPLICATION_TRACKER.md` snapshot if requested.

Use statuses consistently:

- `pending` for submitted applications;
- `process` for interviews/follow-ups;
- `rejected` for rejections;
- `skipped` for roles intentionally not pursued;
- `closed` for closed vacancies.

## Final Response

Keep the final response short:

- branch name;
- files changed;
- PDFs compiled, including both CV variants and the cover letter;
- recommendation or next action;
- any role-specific risk.
