---
name: codex-fix
description: Use OpenAI Codex to fix a bug or issue in code. Trigger on "/codex fix" or "/codex-fix".
---

# Codex Fix

Delegates a bug fix task to OpenAI Codex CLI in full-auto mode.

## Invocation

```
/codex fix <description|file_path>
/codex-fix <description|file_path>
```

## Workflow

1. Take the target from `$ARGUMENTS` (a bug description or file path)
2. Build the prompt: `"Fix this issue: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Fix this issue: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
