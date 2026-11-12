---
name: clean-worktree
description: Clean a worktree shared by parallel Claude Code sessions. Attribute every uncommitted change to the session that made it, leave live sessions' work alone, and resolve the leftovers of closed sessions one semantic group at a time with the user. Use when asked to clean or tidy the worktree, find stale or forgotten edits, or sort out which session owns which changes.
---
Several Claude sessions edit this worktree at once. A live session owns its changes and its git workflow, so ignore them. Handle only what closed sessions (or nobody) left behind, and only once the user has decided what to do with it.

## Gather

* Git: `git status --porcelain`, `git diff --stat`, `git stash list`, `git worktree list`, `git log @{u}..HEAD`. For each submodule: its pin vs. its HEAD, its own status, whether its HEAD is pushed.
* Live sessions: `ListAgents`, and `~/.claude/sessions/<pid>.json` (`sessionId`, `name`, `status`). A session is live if `kill -0 <pid>` succeeds.
* Transcripts: `~/.claude/projects/<cwd with every / as ->/<sessionId>.jsonl`, their subagents in `<sessionId>/subagents/`. A session's name is its last `customTitle` record.

## Attribute

* A session edited a file if its transcript has an `Edit`/`Write` tool_use on the path, or a `Bash` tool_use that names the path and writes it (`sed -i`, heredoc scripts, `mv`, `rm`, `git checkout`).
* Forks copy their parent's transcript. Dedupe edits by tool_use `id`. An edit found in a live fork and in its closed parent belongs to the live one.
* Transcript timestamps are UTC, file mtimes local. If a file's mtime matches no edit, a running script (a sync loop, a generator) or the user wrote it.
* One file can hold several sessions' hunks. Attribute by hunk (`git diff -U0`, `--word-diff` for long lines), not by file.
* Read the last few assistant messages of each session involved. They say what is still running, what was left uncommitted and why, and who owns what.

## Overview

Group the changes by feature or task, not by file. Show two tables: live-owned groups (owner, files, ignored) and stale groups (what was left, by whom, the evidence). Below them, list any bug or risk you noticed in either kind of group, without fixing it.

## Resolve

* Ask about the stale groups one at a time with `AskUserQuestion`, the recommended option first (commit + push, commit only, restore, drop, leave). Don't ask about live-owned groups.
* Re-check the state right before each action, because peer sessions keep editing. Stage explicit paths or hunks only, never `git add -A`, and follow the repo's commit conventions.
* When you remove a file, fix whatever references it.

## Report

A short table: every group, its owner, and its outcome (ignored, owned by X; committed `<sha>` as confirmed; dropped; left). Then the risks in the live groups, one line each.
