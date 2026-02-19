---
name: codex
description: Run OpenAI Codex CLI in full-auto mode — a lightweight, token-efficient way to delegate coding tasks to Codex. Supports subcommands (review, explain, test, fix) or freeform prompts. Shells out directly via npx instead of using a persistent MCP session. Trigger on "/codex" or when the user asks to "run codex", "ask codex", or "use codex for this".
---

# Codex (Full-Auto)

A lightweight skill that delegates coding tasks to OpenAI's Codex CLI agent in full-auto mode. Instead of running Codex as an MCP server (which maintains persistent sessions and uses more tokens), this skill shells out directly via `npx @openai/codex exec --full-auto`, making it cheaper and faster.

## When to Use This Skill

Use this skill when:
- The user invokes `/codex` with a subcommand or task description
- The user asks to "run codex", "ask codex", or "use codex for this"
- The user wants to delegate a coding task to Codex specifically

## Invocation Format

```
/codex [subcommand] <prompt|file_path>
```

## Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `review` | Code review — find bugs, suggest improvements | `/codex review src/auth.ts` |
| `explain` | Explain how code works | `/codex explain src/utils/parser.js` |
| `test` | Generate tests for a file or module | `/codex test src/services/payment.ts` |
| `fix` | Fix a bug or issue | `/codex fix the login timeout error` |
| *(none)* | Freeform — pass any task directly to Codex | `/codex refactor the db module` |

### Argument Parsing Rules

1. If the first argument matches a subcommand (`review`, `explain`, `test`, `fix`), use it as the mode
2. Everything after the subcommand is the target (file path or description)
3. If the first argument does NOT match a subcommand, treat the entire input as a freeform prompt

### Examples

```bash
# Subcommands
/codex review src/auth.ts
/codex explain src/utils/parser.js
/codex test src/services/payment.ts
/codex fix the race condition in the worker queue

# Freeform (no subcommand)
/codex refactor the database module to use connection pooling
/codex create a REST API endpoint for user registration
/codex add input validation to the signup form
```

## Workflow

### Step 1: Parse Arguments

1. Check if the first argument is a known subcommand (`review`, `explain`, `test`, `fix`)
2. If yes, build a targeted prompt based on the subcommand (see Prompt Templates below)
3. If no, use the raw arguments as-is for a freeform prompt

### Step 2: Build the Prompt

#### Prompt Templates by Subcommand

- **review**: `"Review this code for bugs, security issues, and improvements: <target>"`
- **explain**: `"Explain how this code works in detail: <target>"`
- **test**: `"Write comprehensive tests for this code: <target>"`
- **fix**: `"Fix this issue: <target>"`
- **freeform**: Pass the user's input directly as the prompt

### Step 3: Execute Codex

Run the following command using the Bash tool:

```bash
npx @openai/codex exec --full-auto "<built_prompt>"
```

If the command fails because the current directory is not a git repository, retry with:

```bash
npx @openai/codex exec --full-auto --skip-git-repo-check "<built_prompt>"
```

### Step 4: Return Output

Return the raw output from Codex to the user without modification.

## Why This Approach

Running Codex via `npx exec` is significantly lighter than the MCP integration:

| Approach | Session | Token Usage | Overhead |
|----------|---------|-------------|----------|
| MCP Codex | Persistent thread | Higher — protocol + context | MCP middleware |
| `npx exec` (this skill) | One-shot | Lower — direct execution | Minimal |

Credit: Inspired by [Raza Rafiq's suggestion](https://x.com/RazaRafiq1990) for a lighter Codex integration pattern.
