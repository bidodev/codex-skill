---
name: codex-test
description: Use OpenAI Codex to generate tests for a file or module. Triggers on "/codex test", "/codex-test", or natural language like "ask codex to write tests", "have codex add specs", "codex cover this with tests", "codex generate tests for".
---

# Codex Test

Delegates a test generation task to OpenAI Codex CLI in full-auto mode.

## Trigger Patterns

### Slash commands
```
/codex test <file_path|description>
/codex-test <file_path|description>
```

### Natural language
- "ask codex to write tests for src/services/payment.ts"
- "have codex add specs for the auth module"
- "codex cover this with tests"
- "codex generate tests for the API endpoints"

## Workflow

1. Take the target from `$ARGUMENTS`
2. Build the prompt: `"Write comprehensive tests for this code, covering edge cases, error handling, and happy paths: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Write comprehensive tests for this code, covering edge cases, error handling, and happy paths: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
