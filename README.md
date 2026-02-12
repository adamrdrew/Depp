# DEPP — Domain Expert Pair Programmer

A [cookiecutter](https://cookiecutter.readthedocs.io/) template for creating a persistent, growing Claude Code development partner. DEPP generates a project directory that turns Claude Code from a stateless code assistant into a named AI collaborator with memory, personality, and autonomous learning — one that remembers your last conversation, captures what it learns, and grows more capable over time.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Python 3.8+ with [cookiecutter](https://pypi.org/project/cookiecutter/): `pip install cookiecutter`
- (Optional) A vector memory MCP server for semantic long-term memory

## Usage

```bash
cookiecutter gh:adamrdrew/depp
```

You'll be prompted for:

| Variable | Default | Description |
|---|---|---|
| `agent_name` | Atlas | Your agent's name |
| `project_slug` | (agent_name) | Directory name |
| `agent_personality` | A thoughtful, direct... | Free-text personality description |
| `user_name` | Your Name | Your name |
| `user_github` | yourgithub | Your GitHub username |
| `user_experience` | Experienced developer... | Brief description of your background |
| `development_directory` | ~/Development | Where your code lives |

## What You Get

```
YourAgent/
├── .claude/
│   └── skills/
│       ├── wake-up/SKILL.md        # Start-of-session context restore
│       ├── go-to-sleep/SKILL.md    # End-of-session memory capture
│       ├── create-skills/SKILL.md  # How to create new skills
│       └── vector-memory/SKILL.md  # Semantic memory patterns
├── .mcp.json                       # MCP server config (vector memory)
├── CLAUDE.md                       # Agent personality, rules, and architecture
├── index.md                        # Master index of all notes
├── session-buffer.md               # Short-term memory (last session)
└── notes/
    └── current-state.md            # Long-term project state
```

## Setup

### 1. Point Claude Code at your agent directory

Open Claude Code in the generated directory:

```bash
cd YourAgent
claude
```

Or set it as a Claude Code project directory in your IDE integration.

### 2. Configure vector memory (optional but recommended)

The generated `.mcp.json` has a placeholder for the vector memory MCP server. To enable semantic long-term memory, you need a vector memory MCP server that provides `store_memory`, `search_memories`, `list_recent_memories`, `get_memory_stats`, and `clear_old_memories` tools.

Edit `.mcp.json` in your agent directory to point at your chosen server:

```json
{
  "mcpServers": {
    "vector-memory": {
      "command": "your-vector-memory-server",
      "args": ["--working-dir", "/full/path/to/YourAgent"]
    }
  }
}
```

Without vector memory configured, the agent still works — it just uses the other three memory layers (session buffer, notes, skills). Vector memory adds associative recall across sessions.

### 3. Start a conversation

Your agent will invoke its `wake-up` skill at the start of each session to restore context. On the first run, it will see an empty session buffer and introduce itself fresh.

## Architecture: The 4-Layer Memory System

DEPP agents maintain memory across sessions using four complementary layers:

```
┌─────────────────────────────────────────────────┐
│ Session Buffer (session-buffer.md)               │
│ Short-term. What happened last session.          │
│ Overwritten each session by go-to-sleep.         │
├─────────────────────────────────────────────────┤
│ Notes (notes/)                                   │
│ Long-term structured. Project state, decisions,  │
│ research, architecture. Organised by topic.      │
├─────────────────────────────────────────────────┤
│ Skills (.claude/skills/)                         │
│ Procedural. How to do things. Auto-discovered.   │
├─────────────────────────────────────────────────┤
│ Vector Memory (via MCP server)                   │
│ Long-term associative. Patterns, gotchas,        │
│ insights. Retrieved by meaning. The glue.        │
└─────────────────────────────────────────────────┘
```

- **Session buffer** — Short-term memory. Written at end of each session, read at the start of the next. Captures what happened, decisions made, what's pending.
- **Notes** — Long-term structured knowledge. Project state, architectural decisions, research findings. Organised by topic in `notes/`.
- **Skills** — Procedural knowledge. Reusable how-to documents auto-discovered by Claude Code. The agent creates these autonomously when it learns something reusable.
- **Vector memory** — Long-term associative memory. Stores patterns, gotchas, and insights as vector embeddings. Retrieved by semantic similarity, not keywords. The connective tissue between the other layers.

### Wake/Sleep Cycle

The `wake-up` skill runs at session start: reads the session buffer, index, current state, and searches vector memory for relevant context. The agent orients itself and tells you what it remembers.

The `go-to-sleep` skill runs at session end: writes the session buffer, updates notes, captures new skills, and stores learnings in vector memory. Nothing is lost between sessions.

### Autonomous Learning Triggers

The agent doesn't wait to be told to learn. CLAUDE.md defines triggers that fire automatically:

- Discovered something through trial and error → creates a skill immediately
- Done the same operation twice → creates a skill to prevent a third time
- Significant context established → captures it in notes
- Learnt something small but useful → stores it in vector memory
- Session ending → invokes go-to-sleep without being asked

## Customising Your Agent

### Personality

Edit the `Voice` section in `CLAUDE.md`. The `agent_personality` variable from cookiecutter is a starting point — refine it as you work together. You can give your agent an accent, a communication style, a favourite programming paradigm, whatever makes the collaboration feel right.

### Adding MCP Servers

Edit `.mcp.json` to add more MCP servers (macOS automation, notifications, etc.) and document them in the `Tools` section of `CLAUDE.md`.

### Growing the Knowledge Base

The agent does this autonomously, but you can also:
- Add notes manually to `notes/`
- Create skills manually in `.claude/skills/`
- Edit `index.md` to organise your table of contents
- Edit `current-state.md` to set initial project context

## Credits

Created by [Adam Drew](https://github.com/adamrdrew). Architecture extracted from [Alan](https://github.com/adamrdrew), a persistent Claude Code development partner.

## License

MIT
