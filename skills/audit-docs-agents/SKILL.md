---
name: audit-docs-agents
description: Audit and repair the project's `docs/agents/` Markdown knowledge wiki for broken links, stale or conflicting claims, factual errors, missing cross-references, and obsolete content. Use when asked to audit, clean, validate, or refresh that wiki. Fail if `docs/agents/` is absent; fix high-confidence defects in place, use structured questions for ambiguity, and never delete without explicit user approval.
---

# Audit Docs Agents

Audit the entire wiki, not a sample. Read every Markdown file under `docs/agents/` and verify its claims against the project's source, tests, configuration, and other authoritative files. Edit only the wiki.

## Preflight

1. Confirm that `docs/agents/` exists and is a directory. If absent, report the blocking error through `AskUserQuestion` or the host's equivalent structured question tool, then stop. Never create it silently.
2. Read applicable repository instructions.
3. Inventory every wiki file, heading, link, and referenced file, symbol, command, configuration value, API, or workflow.

## Audit

Check the complete wiki for:

- Broken relative links, missing anchors, orphan pages, and missing cross-links between clearly related pages.
- Stale references to code, behavior, paths, commands, configuration, APIs, dependencies, or workflows.
- Contradictions between pages or between the wiki and authoritative project evidence.
- Factual, technical, naming, and prose errors that change meaning or usability.
- Duplicate, superseded, irrelevant, or unfinished content.

Do not treat age alone as evidence of uselessness. Old experiments and decisions may still explain the current architecture.

## Fix

- Correct high-confidence defects directly and minimally.
- Preserve useful historical rationale. Mark superseded material clearly and link to current information instead of erasing unique context.
- Preserve the existing organization and writing style unless they are the defect.
- Ask the user when intent, historical value, or the correct replacement cannot be established from repository evidence.
- Never change project code or documentation outside `docs/agents/`.

## Deletion Candidates

Use judgment to raise any page or section whose expected future value may not justify keeping it. Duplication, supersession, and irrelevance are signals, not requirements. A candidate may be recent and contain unique knowledge—for example, an abandoned experiment, an isolated dead end, an unused idea, or a note with no downstream implementation or likely reuse. Recency and unique information increase the cost of deletion but do not disqualify candidacy; the user decides.

1. Prepare the complete candidate list with the exact path or section, why its future value appears low, what would be lost, and a recommendation.
2. Present the list concisely, then request approval through the structured question tool one decision at a time. A batch is allowed only when every item shares the same rationale and each item is named explicitly.
3. Delete only explicitly approved items. Keep everything else unchanged.

## User Decisions

- Always use `AskUserQuestion` or the host's equivalent structured question tool, such as `request_user_input`.
- Ask exactly one question at a time, put the recommended option first, and include the evidence and consequence needed to decide.
- Never hide a required decision in ordinary prose.

## Verify and Report

Re-scan links and anchors, re-read changed pages, review the final diff, and confirm that no unapproved deletion occurred. Report the files checked, fixes made, unresolved decisions, and deletion outcomes concisely.
