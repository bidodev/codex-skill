---
name: codex
description: Run OpenAI Codex CLI in full-auto mode — a lightweight, token-efficient way to delegate coding tasks to Codex. Shells out directly via npx instead of using a persistent MCP session. Trigger on "/codex" or when the user asks to "run codex", "ask codex", or "use codex for this".
---

# Codex (Full-Auto)

A lightweight skill that delegates coding tasks to OpenAI's Codex CLI agent in full-auto mode. Instead of running Codex as an MCP server (which maintains persistent sessions and uses more tokens), this skill shells out directly via `npx @openai/codex exec --full-auto`, making it cheaper and faster.

## When to Use This Skill

Use this skill when:
- The user invokes `/codex` with a task description
- The user asks to "run codex", "ask codex", or "use codex for this"
- The user wants to delegate a coding task to Codex specifically

## Invocation Format

```
/codex <prompt>
```

- **prompt** (required): The task or question for Codex to handle.

### Examples

```bash
# Fix a bug
/codex fix the authentication bug in src/auth.ts

# Refactor code
/codex refactor the database module to use connection pooling

# Generate code
/codex create a REST API endpoint for user registration

# Ask a question about the codebase
/codex explain how the payment processing pipeline works
```

## Workflow

### Step 1: Parse the Prompt

Extract the user's task from the arguments passed after `/codex`.

### Step 2: Execute Codex

Run the following command using the Bash tool:

```bash
npx @openai/codex exec --full-auto "<prompt>"
```

If the command fails because the current directory is not a git repository, retry with:

```bash
npx @openai/codex exec --full-auto --skip-git-repo-check "<prompt>"
```

### Step 3: Return Output

Return the raw output from Codex to the user without modification.

## Why This Approach

Running Codex via `npx exec` is significantly lighter than the MCP integration:

| Approach | Session | Token Usage | Overhead |
|----------|---------|-------------|----------|
| MCP Codex | Persistent thread | Higher — protocol + context | MCP middleware |
| `npx exec` (this skill) | One-shot | Lower — direct execution | Minimal |

Credit: Inspired by [Raza Rafiq's suggestion](https://x.com/RazaRafiq1990) for a lighter Codex integration pattern.
