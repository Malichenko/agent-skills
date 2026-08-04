---
name: faang-latex-cv-adapter
description: Adapt an existing LaTeX CV to a specific job description with FAANG-level recruiter judgment. Use when the user asks to tailor, rewrite, improve, review, or adapt a LaTeX CV/resume for a concrete vacancy, especially in this CV repository. Preserve LaTeX structure, commands, formatting, sections, and compile safety while improving relevance, ATS keywords, impact, and recruiter fit.
---

# FAANG LaTeX CV Adapter

Adapt the current LaTeX CV to a concrete job description as an experienced senior recruiter would.

## Core Rules

- Treat the job description as the main source of truth for prioritization.
- Preserve LaTeX structure, commands, environments, macros, packages, spacing, and formatting.
- Do not add or remove sections unless the user explicitly asks and the repository pattern supports it.
- Keep edits tightly scoped to content text, bullets, summaries, competencies, and role emphasis.
- Keep all output LaTeX-safe and ready to compile.
- Avoid generic phrases such as "responsible for" and "worked on".
- Prefer recruiter-oriented, impact-focused language with concrete technologies and outcomes.
- Do not invent experience, employers, education, certifications, language levels, or metrics.
- Reuse real candidate facts from the current CV, `JOB_SEARCH_PROFILE.md`, `raw_cv.txt`, and other local profile files when available.

## Adaptation Workflow

1. Read the current target CV file before editing.
2. Read the job description and identify:
   - target title and seniority;
   - must-have technologies;
   - frontend versus backend emphasis;
   - domain signals;
   - language/location/contract constraints;
   - ATS keywords worth reflecting.
3. Compare the role against the candidate profile.
4. Edit the CV only where it improves fit:
   - Summary: align with the target title, product/domain, frontend seniority, and strongest stack overlap.
   - Core competencies / skills: prioritize job-relevant keywords already supported by the candidate profile.
   - Professional experience: emphasize outcomes, frontend architecture, UI systems, performance, testing, integrations, dashboards, and product impact where relevant.
   - Education/additional information: change only if it clearly improves role fit.
5. Preserve the existing bullet count pattern per company unless the user asks otherwise.
6. Compile the LaTeX document and fix syntax or overflow issues caused by the edits.

## Quality Bar

- The adapted CV should still sound truthful and recognizably like the candidate.
- The top third of the CV should make the match obvious within 10 seconds.
- The CV should be frontend-heavy for frontend roles and should not drift into full-stack positioning unless the vacancy requires it and the candidate evidence supports it.
- If the vacancy is risky because of German level, backend-heavy requirements, or unclear contract setup, call that out before doing major tailoring.

## Output Style

When editing files, report briefly:

- which CV file changed;
- what role/company it was adapted for;
- whether compilation succeeded;
- any remaining risk or assumption.
