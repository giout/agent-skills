---
name: checks
description: Execute commands from a specified file sequentially and report errors for each command. Use when asked to run a file-based verification checklist.
---

# Sequential Checks

Read the commands listed in `[FILE]` and execute them sequentially, preserving their listed order. Wait for each command to finish before starting the next one.

## Workflow

1. Read the file and identify its commands, required working directory, and any documented prerequisites.
2. Run one command at a time. Collect each command's exit status, errors, and reported file or line locations.
3. Continue collecting results after ordinary check failures. If a missing prerequisite prevents later commands from running meaningfully, identify those commands as blocked rather than reporting them as passed.
4. Do not fix errors as part of this workflow unless separately requested.

## Report Format

Include every command in the final report using this structure:

```text
Command executed: <command>
Errors found: <errors and their locations, or "None">
---
Command executed: <command>
Errors found: <errors and their locations, or a blocking reason>
---
```
