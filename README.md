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
/codex <prompt>
```

### Examples

```bash
# Fix a bug
/codex fix the authentication bug in src/auth.ts

# Refactor code
/codex refactor the database module to use connection pooling

# Generate code
/codex create a REST API endpoint for user registration

# Ask a question
/codex explain how the payment processing pipeline works
```

## Why This Instead of MCP?

Running Codex via `npx exec --full-auto` is lighter than the MCP-based integration:

- **No persistent session** — one-shot execution, no thread management
- **Lower token usage** — no MCP protocol overhead or context passing
- **Simpler** — just a shell command

Credit: Inspired by [Raza Rafiq's suggestion](https://x.com/RazaRafiq1990) for a lighter Codex integration pattern.

## License

MIT
