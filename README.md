# Codex Skill

A Claude Code skill that runs OpenAI Codex CLI in full-auto mode — a lightweight, token-efficient way to delegate coding tasks to Codex directly from Claude Code.

Supports both **slash commands** and **natural language**.

## Install

```bash
npx skills install bidodev/codex-skill
```

## Prerequisites

- [OpenAI Codex CLI](https://github.com/openai/codex) must be available (installed globally or via npx)
- An OpenAI API key configured for Codex

## Usage

### Slash commands

```bash
/codex review src/auth.ts
/codex explain src/utils/parser.js
/codex test src/services/payment.ts
/codex fix the race condition in the worker queue
/codex refactor the database module
```

### Natural language

```
ask codex to review src/auth.ts
have codex explain the parser
let codex write tests for the payment module
use codex to fix the login bug
codex should refactor the db module
send this to codex
```

## Subcommands

| Intent | Slash | Natural language examples |
|--------|-------|--------------------------|
| Review | `/codex review` | "ask codex to check this code", "codex find bugs in" |
| Explain | `/codex explain` | "have codex walk me through", "codex how does this work" |
| Test | `/codex test` | "codex write tests for", "have codex add specs" |
| Fix | `/codex fix` | "ask codex to debug", "codex solve this bug" |
| Freeform | `/codex <anything>` | "ask codex to refactor", "let codex handle this" |

## Architecture

```
skills/
├── codex/           ← Orchestrator: detects intent, routes to sub-skills
├── codex-review/    ← Code review via Codex
├── codex-explain/   ← Code explanation via Codex
├── codex-test/      ← Test generation via Codex
└── codex-fix/       ← Bug fixing via Codex
```

## Why This Instead of MCP?

Running Codex via `npx exec --full-auto` is lighter than the MCP-based integration:

- **No persistent session** — one-shot execution, no thread management
- **Lower token usage** — no MCP protocol overhead or context passing
- **Simpler** — just a shell command

Credit: Inspired by [Raza Rafiq's suggestion](https://x.com/RazaRafiq1990) for a lighter Codex integration pattern.

## License

MIT
