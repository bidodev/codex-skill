---
name: codex-test
description: Use OpenAI Codex to generate tests for a file or module. Trigger on "/codex test" or "/codex-test".
---

# Codex Test

Delegates a test generation task to OpenAI Codex CLI in full-auto mode.

## Invocation

```
/codex test <file_path|description>
/codex-test <file_path|description>
```

## Workflow

1. Take the target from `$ARGUMENTS` (a file path or description)
2. Build the prompt: `"Write comprehensive tests for this code, covering edge cases, error handling, and happy paths: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Write comprehensive tests for this code, covering edge cases, error handling, and happy paths: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
