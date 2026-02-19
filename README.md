# Codex Skill

A Claude Code skill that runs OpenAI Codex CLI in full-auto mode — a lightweight, token-efficient way to delegate coding tasks to Codex directly from Claude Code.

## Install

```bash
npx skills install bidodev/codex-skill
```

## Prerequisites

- [OpenAI Codex CLI](https://github.com/openai/codex) must be available (installed globally or via npx)
- An OpenAI API key configured for Codex

## Usage

```
/codex [subcommand] <prompt|file_path>
```

### Subcommands

| Subcommand | Description | Example |
|------------|-------------|---------|
| `review` | Code review — find bugs, suggest improvements | `/codex review src/auth.ts` |
| `explain` | Explain how code works | `/codex explain src/utils/parser.js` |
| `test` | Generate tests for a file or module | `/codex test src/services/payment.ts` |
| `fix` | Fix a bug or issue | `/codex fix the login timeout error` |
| *(none)* | Freeform — pass any task directly to Codex | `/codex refactor the db module` |

### Examples

```bash
# Code review
/codex review src/auth.ts

# Explain code
/codex explain src/utils/parser.js

# Generate tests
/codex test src/services/payment.ts

# Fix a bug
/codex fix the race condition in the worker queue

# Freeform tasks
/codex refactor the database module to use connection pooling
/codex create a REST API endpoint for user registration
```

## Why This Instead of MCP?

Running Codex via `npx exec --full-auto` is lighter than the MCP-based integration:

- **No persistent session** — one-shot execution, no thread management
- **Lower token usage** — no MCP protocol overhead or context passing
- **Simpler** — just a shell command

Credit: Inspired by [Raza Rafiq's suggestion](https://x.com/RazaRafiq1990) for a lighter Codex integration pattern.

## License

MIT
