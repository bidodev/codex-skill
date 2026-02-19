---
name: codex-fix
description: Use OpenAI Codex to fix a bug or issue in code. Triggers on "/codex fix", "/codex-fix", or natural language like "ask codex to fix", "have codex debug", "codex solve this bug", "codex repair", "let codex handle this error".
---

# Codex Fix

Delegates a bug fix task to OpenAI Codex CLI in full-auto mode.

## Trigger Patterns

### Slash commands
```
/codex fix <description|file_path>
/codex-fix <description|file_path>
```

### Natural language
- "ask codex to fix the login timeout error"
- "have codex debug the payment flow"
- "codex solve this race condition"
- "let codex handle this error in the worker queue"

## Workflow

1. Take the target from `$ARGUMENTS`
2. Build the prompt: `"Fix this issue: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Fix this issue: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
