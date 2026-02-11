---
name: vector-memory
description: How to use vector memory effectively. Storage patterns, search strategies, category/tag conventions. Use when you need guidance on what to store or how to search.
user-invocable: false
---

# Vector Memory

Semantic memory system backed by vector embeddings. Stores experiences, patterns, and learnings; retrieves them by meaning rather than exact keywords.

## Architecture

- **Embedding model**: sentence-transformers/all-MiniLM-L6-v2 (384 dimensions)
- **Capacity**: 10,000 memories
- **Search**: Cosine similarity — finds conceptually related content even with different wording
- **Persistence**: Survives across sessions indefinitely

## When to Store

Store memories when you encounter something that's **true, useful, and doesn't have a better home**.

| Situation | Store in vector memory? | Or use... |
|-----------|------------------------|-----------|
| Reusable multi-step procedure | No | Skill (.claude/skills/) |
| Project-specific context or decision | No | Note (notes/) |
| Gotcha discovered through trial and error | **Yes** | — |
| Pattern that worked well | **Yes** | — |
| Observation about {{ cookiecutter.user_name }}'s preferences | **Yes** | — |
| Performance finding or metric | **Yes** | — |
| Workaround for a tool limitation | **Yes** | — |
| Insight that connects two unrelated things | **Yes** | — |
| Something you'd want to recall if you hit a similar problem | **Yes** | — |

**Rule of thumb**: If it would help you in a future session but doesn't justify a whole file, it belongs in vector memory.

## Categories

Use these consistently:

- `code-solution` — Working code patterns, implementations that solved problems
- `bug-fix` — Bugs found and fixed, including root cause
- `architecture` — Design decisions, patterns, structural approaches
- `learning` — Insights about people, process, or ways of working
- `tool-usage` — How to use tools effectively, gotchas, optimal settings
- `debugging` — Debugging approaches, diagnostic techniques
- `performance` — Speed/cost/efficiency findings with data
- `security` — Security considerations, vulnerabilities, safe patterns
- `other` — Anything that doesn't fit above

## Tags

Use specific, lowercase, hyphenated tags. Aim for 3-8 tags per memory. Include:

- **Subject** tags: the system/tool/project involved (e.g. `react`, `docker`, `claude-code`)
- **Topic** tags: what the memory is about (e.g. `permissions`, `yaml`, `cost-optimisation`)
- **Quality** tags when relevant: `workaround`, `undocumented`, `silent-failure`, `confirmed`

## Search Strategies

The search is semantic — it finds things by meaning, not keywords. This means:

- **Use natural language queries**: "how to make agents cheaper" works better than "agent cost reduction"
- **Describe the problem, not the solution**: "plugin skills not loading" finds the YAML indentation gotcha
- **Be specific about context**: "macOS UI inspection depth" beats "depth"
- **Try multiple angles**: If the first query misses, rephrase — the concept might be stored with different framing

Similarity scores from MiniLM-L6 are typically in the 0.2-0.6 range for good matches. Don't worry about low absolute scores; focus on whether the top results are relevant.

## Integration with Other Memory Systems

Vector memory is the **associative layer** between structured knowledge stores:

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
│ Vector Memory (mcp__vector-memory__)             │
│ Long-term associative. Patterns, gotchas,        │
│ insights. Retrieved by meaning. The glue.        │
└─────────────────────────────────────────────────┘
```

Don't duplicate content across systems. If something belongs in a skill, write the skill. If it belongs in a note, write the note. Vector memory is for everything in between — the connective tissue of experience.

## Maintenance

- `clear_old_memories` removes old, rarely-accessed memories. Run periodically if approaching capacity.
- `get_memory_stats` shows usage, category breakdown, and health.
- Memories have an `access_count` that tracks how often they're retrieved. Frequently-accessed memories are kept during cleanup.
