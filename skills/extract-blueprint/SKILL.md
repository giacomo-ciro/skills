---
name: extract-blueprint
description: Reverse-engineer an existing feature from a codebase and write a standalone implementation blueprint covering its architecture, data flow, dependencies, configuration, and constraints. Use when a user wants to understand or reproduce implemented behavior; do not use to plan a feature that does not yet exist.
---

# Extract Blueprint

Reverse-engineer the requested feature from the current codebase and document how it works well enough for another engineer to reproduce it.

## Investigate

1. Locate the feature through its names, user-facing labels, routes, commands, configuration, and tests.
2. Identify every entry point, such as an API route, UI component, event handler, scheduled job, or CLI command.
3. Trace the actual execution and data flow through callers, controllers, services, utilities, external integrations, persistence layers, and schemas.
4. Inspect relevant tests, dependency manifests, configuration files, migrations, and interface definitions.

## Analyze

- Explain the architecture and design patterns that are actually present. Do not force the implementation into a named pattern.
- Record direct dependencies and their declared versions, plus any external services or APIs.
- List required environment-variable names and configuration files, but never include secret values.
- Capture validation, authorization, error handling, edge cases, side effects, and material security or performance constraints.
- Support claims with repository-relative file paths and symbol names. Clearly distinguish verified behavior from inference, and call out gaps that the codebase does not resolve.

## Generate the Blueprint

Create `BLUEPRINT-<feature-name>.md` in the project root unless the user specifies another destination. Use a concise kebab-case feature name in the filename and the following structure:

```markdown
# Implementation Blueprint: <Feature Name>

## 1. Technical Stack and Requirements

- **Core dependencies:** Libraries and declared versions used by the feature.
- **Runtime prerequisites:** Required environment-variable names, services, and configuration files.

## 2. Architecture Map

- **Entry points:** Paths and symbols that initiate the feature.
- **Business logic:** Paths and symbols that implement the behavior.
- **Data model:** Schemas, migrations, interfaces, and persistence details.
- **Data flow:** The end-to-end sequence, including external integrations and side effects.

## 3. Step-by-Step Reproduction

1. **Setup:** Types, dependencies, configuration, and data structures to establish.
2. **Core logic:** Ordered implementation steps, with focused code snippets or pseudocode where they clarify non-obvious behavior.
3. **Integration:** How to connect the feature to the host application and verify it using the discovered tests or observable behavior.

## 4. Critical Logic and Constraints

- Validation and edge cases.
- Authorization and security requirements.
- Error handling and recovery behavior.
- Performance-sensitive decisions and other implementation traps.
```

Keep the document standalone and specific to the observed implementation. Omit irrelevant template bullets rather than filling them with speculation.
