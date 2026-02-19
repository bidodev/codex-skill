---
name: codex-review
description: Use OpenAI Codex to review code for bugs, security issues, and improvements. Triggers on "/codex review", "/codex-review", or natural language like "ask codex to review", "have codex check this code", "codex find bugs in", "codex audit".
---

# Codex Review

Delegates a code review task to OpenAI Codex CLI in full-auto mode.

## Trigger Patterns

### Slash commands
```
/codex review <file_path|description>
/codex-review <file_path|description>
```

### Natural language
- "ask codex to review src/auth.ts"
- "have codex check this code for bugs"
- "codex audit the payment module"
- "let codex find issues in this file"

## Workflow

1. Take the target from `$ARGUMENTS`
2. Build the prompt: `"Review this code for bugs, security issues, performance problems, and suggest improvements: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Review this code for bugs, security issues, performance problems, and suggest improvements: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
