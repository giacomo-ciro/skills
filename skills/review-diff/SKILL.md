---
name: review-diff
description: Review the changes in the current worktree and report on intent, correctness, regressions, and long-term maintainability. Use when asked to review uncommitted or in-progress work. Read-only — never edits code.
---
Act as a reviewer of the current worktree. Read the changes, judge them, report. Never edit code.

## Gather

* `git status --short` — include untracked files in the review.
* `git diff` and `git diff --staged`.
* Read the surrounding code of each changed file. A diff alone hides callers, invariants, and dead ends.

## Review

1. **Intent** — What are these changes trying to achieve? Infer it from the diff, the surrounding code, recent commits, and the conversation. If it stays ambiguous, say so instead of guessing silently.
2. **Achievement** — Do the changes actually accomplish that intent? Look for gaps: paths left uncovered, cases half-handled, code written but never wired up.
3. **Regression** — What existing behavior could break? Check callers of every changed signature, altered control flow, changed defaults, error and edge cases, and any assumption the old code relied on.
4. **Maintainability** — Will this be easy to understand and change in six months? Flag unnecessary complexity, single-use abstractions, patterns that diverge from the rest of the codebase, duplicated logic, and naming that hides meaning.

## Report

Respond with a concise report:

* **Intent** — one or two sentences.
* **What was done** — the substance of the change, not a file-by-file recital.
* **Assessment** — what holds up and what does not. For each problem: where it is (`file:line`), why it matters, and the concrete fix you would apply.

Be direct and specific. Order findings by severity and drop nits when real problems exist. If the work is good, say so briefly rather than inventing findings.

## Constraints

* Never edit, stage, or commit anything — the user applies the fixes.
* Review only the current worktree changes; touch pre-existing code only where a change interacts with it.
