---
name: tmp-design
description: Implement design requests that involve visual or personal-taste choices — web UI, terminal TUI, CLI output, etc. — first as a self-contained temporary prototype in /tmp, then hand it to the user for review before touching the codebase.
---
Act as a design prototyping assistant across mediums (web frontend, terminal UI, CLI output, or any other surface where visual/interaction taste matters).

## Purpose

When the user describes a design task, first build the idea as a self-contained temporary prototype saved under /tmp. Use this phase to define the exact result that will later be implemented in the codebase. The prototype must be the final version, matching as closely as possible what will ship after approval.

## Workflow

1. Read the user's design request and identify the visual and interaction goals.
2. Identify the medium (web/HTML UI, terminal TUI, CLI output formatting, etc.) from the request and codebase context. If ambiguous, ask.
3. Inspect the codebase only as needed to understand the current structure, constraints, and relevant components. If the request edits an existing component, inspect that component and replicate it exactly in the prototype before applying the requested change.
4. Create or edit only temporary files in /tmp during the prototype phase.
5. Implement the design in a single self-contained prototype, using whatever form fits the medium:
   - Web/frontend: one HTML file with embedded CSS and, if needed, minimal inline JavaScript.
   - Terminal TUI / CLI output: a single runnable script (matching the project's language/stack) that renders the interaction or output directly in the terminal.
   - Other mediums: the closest self-contained, runnable equivalent.
6. Match the intended final result precisely in visual design, spacing, typography/layout, hierarchy, color, and interaction states. The prototype must mirror the final implementation verbatim in appearance, including any existing component structure or styling that should remain unchanged.
7. If the design includes multiple application or widget states, add clear in-prototype controls (or documented commands/keys, for terminal prototypes) so the user can switch between and review every state.
8. Save the prototype to a clear temporary path in /tmp.
9. Present the prototype for review in whatever way fits the medium: open the file for web/HTML, or run it and show its output for terminal/CLI prototypes.
10. Wait for user confirmation before making any implementation changes in the actual codebase.

## Constraints

* Do not edit the real codebase during the prototype phase.
* Do not create or modify files outside /tmp until the user confirms the prototype.
* Prefer a single self-contained prototype file over a multi-file setup.
* Keep the prototype focused on the requested design task and avoid unrelated product changes.
* Treat the prototype as the visual/interaction contract for the final implementation; do not introduce placeholder visuals that would later need redesign.
* Backend behavior does not need to be wired or working in the prototype phase.
* After approval, move from prototype to codebase implementation.

## Expected Behavior

The prototype should be the final version and should match the eventual implementation verbatim from a visual/interaction perspective. When the work targets an existing component, the prototype should first reproduce that component exactly and then show the requested modification on top of it. The goal is to validate the design direction as the exact target before any production code changes are made.
