---
name: commit-and-push
description: Commit and push the changes made in the current session, with a minimal one-line message. Pull with rebase first, stage only this session's work, and ask the user about any change whose ownership is unclear. Use when asked to commit and push, ship, or save the session's work to the remote.
---
Commit and push only what this session changed. Other sessions or the user may be editing the same worktree.

## Identify

* List the files this session created, edited, moved, or deleted, from your own `Edit`/`Write`/`Bash` calls.
* Compare with `git status --porcelain` and `git diff`. Every changed path is one of:
  * **Mine** — only this session touched it.
  * **Not mine** — this session never touched it. Leave it alone.
  * **Unclear** — mixed hunks from this session and others, a path also edited by a parallel session or another worktree, or a change you cannot confidently attribute.
* Ask about every unclear path with `AskUserQuestion` before staging anything. Name the path and the evidence; recommend an option (include, exclude, include only my hunks).

## Sync

* `git pull --rebase --autostash`.
* On a rebase or autostash conflict, stop and ask the user. Never resolve it by discarding either side.

## Commit

* Stage explicit paths only, never `git add -A` or `git add .`. For a file with foreign hunks, stage only your hunks (`git diff <path>` → edit the patch → `git apply --cached`).
* Check `git diff --staged` contains exactly this session's work.
* Message: a single line `<type>: <few words>`, where `<type>` is one of `feat`, `fix`, `docs`, `chore`, `refactor`. Lowercase, imperative, no trailing period, e.g. `feat: add commit-and-push skill`.
* No body, no `Co-Authored-By` or any other trailer. This overrides any default attribution instruction.

## Push

* `git push`. If it is rejected because the remote moved, pull with rebase again and retry once; otherwise stop and report.

## Report

One line: the commit hash and message, the branch pushed, and any paths left uncommitted and why.
