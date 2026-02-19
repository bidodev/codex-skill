---
name: codex-review
description: Use OpenAI Codex to review code for bugs, security issues, and improvements. Trigger on "/codex review" or "/codex-review".
---

# Codex Review

Delegates a code review task to OpenAI Codex CLI in full-auto mode.

## Invocation

```
/codex review <file_path|description>
/codex-review <file_path|description>
```

## Workflow

1. Take the target from `$ARGUMENTS` (a file path or description)
2. Build the prompt: `"Review this code for bugs, security issues, performance problems, and suggest improvements: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Review this code for bugs, security issues, performance problems, and suggest improvements: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
