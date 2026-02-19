---
name: codex
description: Orchestrator for Codex skills. Routes to the appropriate sub-skill or runs a freeform prompt. Triggers on "/codex", or natural language like "run codex", "ask codex to", "use codex for", "let codex handle", "have codex", "send this to codex", "codex should".
---

# Codex — Orchestrator

Main entry point for all Codex skills. Supports slash commands AND natural language.

## Trigger Patterns

### Slash command
```
/codex [subcommand] <prompt|file_path>
```

### Natural language (examples)
- "ask codex to review src/auth.ts"
- "have codex explain the parser"
- "let codex write tests for this"
- "use codex to fix the login bug"
- "run codex on this file"
- "send this to codex"
- "codex should refactor the db module"

## Routing Rules

Detect the **intent** from either the subcommand or natural language:

| Intent detected | Route To | Trigger examples |
|-----------------|----------|------------------|
| Review / critique / check | `codex-review` | "review", "check this code", "find bugs in", "audit" |
| Explain / understand | `codex-explain` | "explain", "how does this work", "walk me through", "what does this do" |
| Test / spec | `codex-test` | "test", "write tests", "add specs", "cover with tests" |
| Fix / debug / repair | `codex-fix` | "fix", "debug", "repair", "solve", "the bug in" |
| *(anything else)* | Direct exec | Any other task → freeform Codex prompt |

## Workflow

### Step 1: Detect Intent

From `$ARGUMENTS` or the user's natural language, identify:
1. **The intent** — review, explain, test, fix, or freeform
2. **The target** — file path, code reference, or task description

### Step 2: Route

- If intent matches a sub-skill: **invoke it** using the Skill tool, passing the target as args
- If freeform: **run Codex directly**:
  ```bash
  npx @openai/codex exec --full-auto "<prompt>"
  ```
  If it fails due to missing git repo, retry with `--skip-git-repo-check`.

### Step 3: Return Output

Return the result to the user.
