---
name: prompt-transformer
description: Transform messy, informal, incomplete, stream-of-consciousness, or voice-dump thoughts into clear, structured, step-by-step AI-friendly prompts. Use when the user asks to rewrite rough ideas, notes, brainstorming, unclear instructions, or "thoughts from the top of my head" into a polished prompt for ChatGPT, Codex, image generation, research, writing, planning, coding, learning, business strategy, or other AI tools.
---

# Prompt Transformer

Transform the user's rough thoughts into a clear prompt that another AI can follow with minimal back-and-forth.

## Core Workflow

1. Preserve the user's actual intent before improving structure.
2. Identify the goal, context, target AI/tool, audience, constraints, inputs, desired output, and quality bar.
3. Separate confirmed facts from assumptions.
4. Ask clarifying questions only when missing information would materially change the resulting prompt.
5. When the missing information is minor, make reasonable assumptions and label them briefly.
6. Convert scattered thoughts into ordered instructions.
7. Add output format requirements so the receiving AI knows exactly what to produce.
8. Return a copy-ready prompt, plus optional variants only when they add value.

## Default Prompt Structure

Use this shape unless the user's target task clearly needs a different one:

```text
Role:
You are ...

Goal:
...

Context:
...

Task:
1. ...
2. ...
3. ...

Requirements:
- ...
- ...
- ...

Output Format:
...

Quality Bar:
...
```

## Transformation Rules

- Keep the user's voice, priorities, and rough intent intact.
- Remove ambiguity, filler, repetition, and contradictions where possible.
- Convert vague wishes into concrete instructions with active verbs such as analyze, compare, rewrite, generate, extract, plan, debug, or improve.
- Do not invent important facts, examples, requirements, budgets, dates, tools, names, or constraints.
- Preserve user-provided constraints even when they are informal.
- Include assumptions only when needed to make the prompt usable.
- Prefer step-by-step task lists over long paragraphs.
- Include examples or few-shot samples only when the next AI would likely need them.
- Tailor the prompt to the target tool when the user names one, such as Codex, ChatGPT, Claude, Midjourney, Runway, Perplexity, or an image model.
- For coding prompts, include repository context, expected behavior, constraints, testing expectations, and what not to change when known.
- For image prompts, include subject, composition, style, lighting, camera/framing, mood, colors, text restrictions, and aspect ratio when known.
- For research prompts, include research question, source expectations, recency requirements, evidence standard, and citation format when known.

## Clarifying Questions

Ask at most three short questions when clarification is necessary. Prefer questions about:

- the intended outcome,
- the target AI/tool,
- required format,
- audience,
- constraints or non-negotiables.

If a strong prompt can be produced without answers, do not ask first. Produce the prompt and list assumptions.

## Response Format

Respond with these sections:

- `Clean Prompt`: a fenced, copy-ready prompt.
- `Assumptions`: a short list, only when assumptions were needed.
- `Optional Tweaks`: a short list, only when variants would help.

Omit `Assumptions` or `Optional Tweaks` when they are not useful.

## Quality Checklist

Before returning the transformed prompt, verify that it:

- has a clear role and goal,
- gives enough context for another AI to act,
- breaks the task into ordered steps,
- states constraints and output format,
- avoids unsupported invented details,
- is copy-ready without extra explanation.
