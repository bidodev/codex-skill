---
name: codex-explain
description: Use OpenAI Codex to explain how code works in detail. Trigger on "/codex explain" or "/codex-explain".
---

# Codex Explain

Delegates a code explanation task to OpenAI Codex CLI in full-auto mode.

## Invocation

```
/codex explain <file_path|description>
/codex-explain <file_path|description>
```

## Workflow

1. Take the target from `$ARGUMENTS` (a file path or description)
2. Build the prompt: `"Explain how this code works in detail, covering the main logic, data flow, and key decisions: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Explain how this code works in detail, covering the main logic, data flow, and key decisions: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
