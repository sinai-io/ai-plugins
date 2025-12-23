# Sinai AI Plugins

Collection of AI plugins and extensions developed by Sinai RnD for Claude Code and other MCP-enabled tools.

## Installation

```bash
claude plugin marketplace add sinai-io/ai-plugins
```

## Plugins

### tliner

> 🌟 AI's diary: track the work with the markdown log

Timeline tracking for AI agent work. Logs decisions, implementations, and context as searchable ***markdown*** files.

**Install:**
```bash
uvx --from tliner@latest tliner-install
```

**Usage:**
- `/tliner:save` — save current work to timeline
- `/tliner:load` — restore context from past work
- `/tliner:report` — generate progress summary

📖 [Docs on PyPi](https://pypi.org/project/tliner/)
