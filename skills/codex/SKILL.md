---
name: codex
description: Orchestrator for Codex skills. Routes to the appropriate sub-skill (codex-review, codex-explain, codex-test, codex-fix) or runs a freeform prompt. Trigger on "/codex" or when the user asks to "run codex", "ask codex", or "use codex for this".
---

# Codex — Orchestrator

This is the main entry point for all Codex skills. It parses the user's input and routes to the appropriate sub-skill, or runs a freeform Codex prompt.

## Invocation Format

```
/codex [subcommand] <prompt|file_path>
```

## Routing Rules

Parse the first argument and route accordingly:

| First Argument | Route To | Action |
|----------------|----------|--------|
| `review` | `/codex-review` | Invoke the `codex-review` skill with remaining args |
| `explain` | `/codex-explain` | Invoke the `codex-explain` skill with remaining args |
| `test` | `/codex-test` | Invoke the `codex-test` skill with remaining args |
| `fix` | `/codex-fix` | Invoke the `codex-fix` skill with remaining args |
| *(anything else)* | Direct exec | Run Codex directly with the full input as a freeform prompt |

## Workflow

### Step 1: Parse the First Argument

Extract the first word from `$ARGUMENTS`.

### Step 2: Route

- If the first word is `review`, `explain`, `test`, or `fix`:
  **Invoke the matching skill** using the Skill tool. Pass everything after the subcommand as the skill's arguments.
  Example: `/codex review src/auth.ts` → invoke skill `codex-review` with args `src/auth.ts`

- If the first word is NOT a known subcommand:
  **Run Codex directly** with the full `$ARGUMENTS` as a freeform prompt:
  ```bash
  npx @openai/codex exec --full-auto "$ARGUMENTS"
  ```
  If it fails due to missing git repo, retry with `--skip-git-repo-check`.

### Step 3: Return Output

Return the result from the sub-skill or from Codex directly to the user.
