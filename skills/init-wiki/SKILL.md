---
name: init-wiki
description: Bootstrap a project-specific Markdown wiki for agents in `docs/agents/` (one file per topic), add its index and maintenance rules to the project's AGENTS.md, and optionally seed it by surveying the project, as a whole or on a focus the user names. Use when asked to init, set up, create, or bootstrap an agent wiki, a knowledge base, or `docs/agents/` in a project; use `audit-docs-agents` instead to repair an existing one.
---

# Init Wiki

Set up a living wiki that agents read before working on a topic and update with every change: `docs/agents/<topic>.md`, one page per topic, indexed in the project's agent instructions file. Its purpose is to keep what the code alone does not tell, so the next session starts from what the previous ones learned.

## Preflight

1. Read the project's agent instructions. The target file is `AGENTS.md` at the repository root; if only `CLAUDE.md` exists, use it; if `CLAUDE.md` only imports `AGENTS.md`, edit `AGENTS.md`. If neither exists, ask whether to create `AGENTS.md`.
2. If `docs/agents/` already exists or the instructions already index a wiki, stop and ask whether to extend it or leave it; never overwrite existing pages.
3. Ask how to initialize it, with `AskUserQuestion` (or the host's equivalent structured question tool), in one question:
   - **Whole project**: survey the entire project thoroughly and organize what is learned into topics.
   - **Specific focus**: survey only what the user names (areas, directories, workflows, questions). If the answer does not already say what, ask for it in a follow-up question.
   - **Start empty**: create the structure and the instructions only; pages are written as work happens.

## Wire the Instructions

Create `docs/agents/` and add a `## Docs` section to the instructions file, after the existing project rules. Match the file's existing tone and heading levels; adapt the wording, not the substance:

```markdown
## Docs

Topic docs live in `docs/agents/`, one file per topic. Read the relevant file before working on its topic, and keep it up to date with your changes.

- Update a topic's page in the same change that alters what it describes.
- A new topic gets its own page and a one-line entry in the index below.
- Write what the code alone does not tell: why things are as they are, decisions and who took them (with the date), measured results and how they were obtained, setup steps, gotchas, and dead ends.
- Never silently delete what was dropped: mark it with the date and the reason, point to what replaced it, and move its entry to the deprecated group.
- Keep the index in sync with the files in `docs/agents/`.

### <Group>
- [<topic>.md](docs/agents/<topic>.md): <what it covers, in one line>.

### Archived & Reference (Deprecated)
- [<topic>.md](docs/agents/<topic>.md): <what it was> (dropped <YYYY-MM-DD>).
```

Group the index by area (e.g. architecture, data, evaluation, infrastructure), one line per page that tells an agent whether to open it. With **Start empty**, keep the section without entries. Leave out the deprecated group until it has a page.

## Seed the Pages

Skip this section with **Start empty**.

1. **Survey.** Read the project for real: README, instructions, configuration, entry points, source modules, scripts, tests, CI, environment and dependency files, existing notes, and recent git history for decisions and dropped work. Cover everything with **Whole project**; with **Specific focus**, only what the focus needs. In a large project, reading may be split across parallel subagents, but verify each claim you write.
2. **Propose topics.** Split the knowledge into topics an agent would look up before a task: each a coherent unit of work (a subsystem, a pipeline stage, a workflow, an environment or machine, an experiment), not one page per file. Present the proposed pages with their one-line descriptions and confirm the split with the structured question tool before writing.
3. **Write each page** in the style below, then fill the index.

A useful wiki is often a few dense pages, not many thin ones. Do not create a page that only restates the code.

## Page Style

- Name pages `<topic>.md` in kebab-case, flat in `docs/agents/`.
- Open with a `#` title and a short paragraph: what the topic covers and its entry points (scripts, modules, commands, configs).
- State facts precisely: paths, commands, config keys, and symbols in backticks; numbers with their unit and with where and how they were measured.
- Attribute decisions: who took them, when, and why, e.g. "(the user, 2026-11-01: <reason>)". A fact true only from a date on says so ("since 2026-11-14").
- Put the current state first. Keep history only where it explains the present or prevents repeating a dead end; mark it with its date and say what superseded it.
- Separate what was verified from what is suspected: write "suspected, not confirmed" rather than presenting a guess as fact.
- Link related pages with relative links (`[data.md](data.md)`) instead of duplicating their content.
- Use plain prose and flat lists; use headings to make long pages scannable. No filler, no generic best practices, no copies of code.
- Write only what the survey established. If something important cannot be determined, ask the user, or leave it out and report it.

## Verify and Report

Check that every relative link and anchor resolves, that the index lists exactly the files in `docs/agents/`, and that every page was re-read against the sources it cites. Do not stage or commit unless asked. Report the instructions file changed, the pages created with one line each, and anything left unresolved.
