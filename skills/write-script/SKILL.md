---
name: write-script
description: Write or revise self-contained Python to adhere to user taste and style preference. Use whenever creating or modifying a Python script.
---

# Writing Scripts

Every script must be self-contained, minimal, and runnable with no command-line arguments. Optimize for the person who will read, debug, and extend it.

## Structure

A script has three sections:

1. Imports, `REPO_ROOT`, and helper functions.
2. `main()`, which receives every tunable value and orchestrates the work.
3. An `if __name__ == "__main__":` block that defines configuration as `UPPER_SNAKE_CASE` constants and passes it to `main()` by keyword.

```python
from pathlib import Path


REPO_ROOT = Path(__file__).resolve().parents[1]


def load_records(input_path: Path, limit: int) -> list[dict]:
    ...


def process_records(records: list[dict]) -> list[dict]:
    ...


def save_records(records: list[dict], output_path: Path) -> None:
    ...


def main(input_path: Path, output_path: Path, limit: int) -> None:
    # Load and transform the records.
    records = load_records(input_path, limit)
    processed_records = process_records(records)

    # Save the result.
    save_records(processed_records, output_path)


if __name__ == "__main__":
    INPUT_PATH = REPO_ROOT / "data" / "input.json"
    OUTPUT_PATH = REPO_ROOT / "outputs" / "results.json"
    LIMIT = 100

    main(
        input_path=INPUT_PATH,
        output_path=OUTPUT_PATH,
        limit=LIMIT,
    )
```

## Rules

### Configuration

- Never use `argparse`, `click`, `typer`, `sys.argv`, or another argument parser.
- Put every tunable value in the entry-point block as an `UPPER_SNAKE_CASE` constant.
- Pass every constant to `main()` by keyword. The `main()` signature is the script's configuration contract.
- Add a short inline comment only when a value is not self-explanatory.
- Define `REPO_ROOT = Path(__file__).resolve().parents[1]` at module level and build paths from it. Adjust the parent depth only when the script is not directly under `scripts/`; never hardcode machine-specific absolute paths.

### Orchestration

- Make `main()` a readable, top-to-bottom outline of the workflow. Keep implementation details in helpers.
- Give each step a clear name, minimal inputs, and minimal outputs. Do not pass unrelated values through intermediate steps.
- Use blank lines and short comments to separate steps only when their purpose is not already clear from the calls.
- Make `main()` depend on its parameters, not module-level configuration.

### Functions, Imports, and Failures

- Give each function one responsibility. Reuse existing project functions instead of duplicating them.
- Prefer named intermediate values and straightforward control flow over dense expressions.
- Keep the implementation as simple as the intended use permits. Do not add classes, abstractions, or indirection without a concrete need.
- Import required project objects from their defining modules instead of recreating or wrapping them locally.
- Keep imports at module level. Use lazy imports only when required for correctness or explicitly requested.
- Assume the script's intended inputs and environment. Do not add fallbacks, retries, substitute defaults, degraded modes, or speculative error handling. We are not writing production code.
- Let missing dependencies, invalid inputs, and violated assumptions fail immediately. Do not catch exceptions unless the task explicitly requires handling them.
- Keep helpers in the script while they are small and script-specific. Move shared or substantial logic into the project's source package.
