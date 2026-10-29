---
name: write-plan
description: Create and iteratively refine a concise, living plan file (PLAN_<topic>.md) that shows the intended outcome, end-to-end direction, settled decisions, and open issues at a glance. Use when the user asks to plan, design, or determine an approach before implementation; do not use to execute an existing plan.
---

# Write Plan

Create a compact, living plan file (`PLAN_<topic>.md`, e.g., `PLAN_new_feature.md`) that gives the user an immediate end-to-end view of the intended work and becomes more precise as understanding improves. The plan is a shared map, not a detailed implementation specification.

## Start with the Skeleton

- Read the request and inspect the immediately relevant project artifacts just far enough to understand the starting point.
- Create the plan file in the project root immediately, naming it with a meaningful subtitle (e.g., `PLAN_new_feature.md`) so parallel planning runs do not overwrite each other. If an existing plan file belongs to the current work, revise it in place. If it belongs to unrelated work, ask before replacing it.
- Write a simple provisional path from start to finish even when details remain unresolved. Mark uncertainty instead of inventing an answer.

Use this minimal shape:

```markdown
# Plan: <short name>

Status: Draft

## Goal

- <intended outcome and success criteria>

## Direction

- <overall structure or architecture>

## Path

- <first meaningful stage>
- <next meaningful stage>
- <final outcome or verification>

## Decisions

- <settled decision>

## Open issues

- <unresolved decision, risk, or concern that could change the direction>
```

Mark the plan as `Draft` while questions remain and `Ready` only after the final review. Omit any section that has no useful content; never retain placeholder text.

## Refine Together

Ask exactly one question at a time and wait for the answer. Use the available interactive question mechanism when one exists; otherwise ask directly in the conversation. Give only the context needed to decide, and present a recommendation when it helps. Never end a turn with a questionnaire or a backlog of questions.

Work in two passes:

1. **Ground the direction.** Resolve the outcome, scope boundaries, major components, architectural direction, constraints, and other choices that determine the overall shape. Resolve foundational decisions before dependent ones.
2. **Challenge the plan.** Once the direction is coherent, inspect more deeply and test it for incorrect assumptions, logical gaps, failure modes, edge cases, integration problems, and material operational or security concerns.

Resolve discoverable facts from available evidence instead of asking the user. When intent is unclear, evidence is inconclusive, or a choice could materially change the outcome, ask rather than assume. Update the plan file whenever new information changes the direction, a decision, or an open issue.

## Keep the Plan Glanceable

- Use plain Markdown headings, flat bullet lists, short lines, and blank lines between distinct ideas.
- Put one decision, concern, or step on each line.
- Keep the end-to-end path in execution order, but do not expand it into mechanical tasks.
- State settled decisions directly in **Decisions** so they are visibly pinned down and cannot be mistaken for suggestions.
- Include only durable, non-trivial information needed to understand the direction and execute it correctly.
- Keep decisions concise. Record rationale only when it prevents the decision from being misunderstood or revisited without cause.
- Keep unresolved matters in **Open issues**. Remove them when resolved and preserve the resulting decision instead.
- Replace stale statements rather than accumulating history. The plan file must always describe the current understanding, not the conversation that produced it.
- Omit code, exhaustive file lists, mechanical implementation steps, easily inferred details, generic best practices, and empty boilerplate sections.
- Do not use tables, badges, callouts, decorative formatting, canned introductions, or filler text.
- Avoid nested lists and long paragraphs. Use them only when flattening the content would make it ambiguous.
- Cite source locations only when they are essential anchors.

## Finish

When the user says the plan is sufficient, perform one final review against the available evidence. If a material concern, contradiction, unresolved decision, or logical gap remains, raise it one question at a time and continue updating the plan. If none remain, mark the plan `Ready` and report that it is ready for implementation.

During planning, modify only the plan file; investigation of all other artifacts is read-only. Do not implement or change code, configuration, tests, documentation, or generated artifacts until the user explicitly authorizes implementation with language such as “proceed,” “approved,” or “implement.” Do not infer authorization from silence, praise, or a statement that the plan is merely sufficient.
