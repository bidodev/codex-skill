---
name: codex-explain
description: Use OpenAI Codex to explain how code works in detail. Triggers on "/codex explain", "/codex-explain", or natural language like "ask codex to explain", "have codex walk me through", "codex what does this do", "codex how does this work".
---

# Codex Explain

Delegates a code explanation task to OpenAI Codex CLI in full-auto mode.

## Trigger Patterns

### Slash commands
```
/codex explain <file_path|description>
/codex-explain <file_path|description>
```

### Natural language
- "ask codex to explain src/utils/parser.js"
- "have codex walk me through the auth flow"
- "codex what does this function do"
- "codex how does the scheduler work"

## Workflow

1. Take the target from `$ARGUMENTS`
2. Build the prompt: `"Explain how this code works in detail, covering the main logic, data flow, and key decisions: <target>"`
3. Execute:
   ```bash
   npx @openai/codex exec --full-auto "Explain how this code works in detail, covering the main logic, data flow, and key decisions: <target>"
   ```
4. If it fails due to missing git repo, retry with `--skip-git-repo-check`
5. Return the raw output to the user
