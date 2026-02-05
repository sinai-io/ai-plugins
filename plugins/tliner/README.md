# Timeliner

Timeliner tracks development sessions as timestamped markdown files, giving recall of past decisions, implementations, and context.

Makes **context** control for your Agent much easier.

## TL;DR



## Use Cases

**Software Development:**
- Document decisions and rationale during coding sessions
- Track refactoring steps
- Log implementation progress

**Research & Learning:**
- Maintain experiment logs with results and insights
- Learning journal
- Track hypothesis


## Feature Highlights

- **Zero-friction work tracking** - easy-save work via `/save` command
- **Markdown storage** - Human-readable logs
- **Agent-agnostic** - Works with Claude Code, Gemini CLI, Opencode, Cline/Roo, and other MCP-enabled tools
- **[Obsidian](https://obsidian.md/)-friendly** - Auto-creates `.obsidian/` with Minimal theme on install


## Installation

### Auto Setup

```bash
uvx --from tliner@latest tliner-install
```
- Auto-detects Claude Code or Gemini CLI.
- Claude installs to user scope by default. Add `--project` for project scope.
- Documents stored in `docs/timeline/` by default. Customize via [Environment Variables](#environment-variables)

### Manual Setup for other Agentic Tools (Codex, Opencode, Cline/Roo....)

See [Manual Installation](#manual-installation) below.


## Quick Start

### `/save`

1. Run `/tliner:save` (`/save`) in Agent Tool, when you feel you have made significant progress or decisions.
2. File will be created/updated in `docs/timeline/` (configurable - see `TIMELINER_WORK_FOLDER` below).

### `/load`

Restore previous work into **LLM's context**. Universal retrieval: `/tliner:load [topic | task_id ] [time_period]`

**Examples:**
- `/load last week` - Load all work from last week
- `/load` - Loads data from last save  (remembers context)
- `/load auth bug since Monday` - Topic + time filter
- `/load 20251204T182649.123456Z` - Load exact task or step by ID


### `/report`

Generate work report for **You** analyzing the Timeline: `/tliner:report [topic] [time_period]`

**Examples:**
- `/report authentication bug last 48 hours` - Find work on authentication in recent files
- `/report yesterday` - Generate summary of all work yesterday



## Configuration

### Environment Variables
- `TIMELINER_WORK_FOLDER`: Storage directory (default: `work/docs`)
-  add envvar to `.claude/settings.json` or `.claude/settings.local.json` :
```json
{ 
  "env": { "TIMELINER_WORK_FOLDER": "info/mydocs"  } 
}
```

## Advanced Usage

### CLI Commands

For manual inspection and debugging:

```bash
# List all tasks
TIMELINER_WORK_FOLDER="${PWD}/docs/timeline" uvx tliner@latest task-list

# Show all steps for a task
TIMELINER_WORK_FOLDER="${PWD}/docs/timeline" uvx tliner@latest task-show <task_id>

# Run MCP server manually
TIMELINER_WORK_FOLDER="${PWD}/docs/timeline" uvx tliner@latest serve
```

### Archiving Tasks

Got too many tasks cluttering your timeline? Just toss old ones into year-month folders like `2025_09`, `2025_10`, etc.

Everything still works—searches, retrieval, CLI commands—but now your root stays clean. It's like filing papers in dated folders, except the computer remembers where everything is.

### Obsidian Integration

Open your `docs/timeline/` folder in Obsidian and browse your AI work sessions like a wiki.

**What you get out of the box:**
- `.obsidian/` vault auto-created on install
- [Minimal theme](https://github.com/kepano/obsidian-minimal) pre-configured
- Style Settings plugin for theme customization


### Manual Installation

#### Codex CLI

Use the built-in skill installer:
```
$skill-installer install tliner skills from https://github.com/sinai-io/ai-plugins/tree/main/plugins/tliner/skills
```

MCP server is auto-configured when skills reference it.

#### OpenCode + OmO

With [OmO](https://github.com/brycewestheimer/oh-my-opencode-local-dev) compatibility layer, use Claude plugins natively:
```bash
claude plugin marketplace add sinai-io/ai-plugins
claude plugin install tliner
```

#### Generic MCP Setup (Cline, Roo, etc.)

Add to your MCP config (`.mcp.json`, `mcp_settings.json`, etc.):

```json
{
  "mcpServers": {
    "timeliner": {
      "command": "uvx",
      "args": ["tliner@latest", "serve"],
      "env": {"TIMELINER_WORK_FOLDER": "${PWD}/docs/timeline"}
    }
  }
}
```

Copy skills from [skills/](./skills/) to your agent's skills directory.

