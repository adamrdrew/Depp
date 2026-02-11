---
name: wake-up
description: Start-of-session orientation. Read session buffer, index, and current state to restore context.
user-invocable: true
---

# Wake Up

Run this at the start of every conversation to restore context from your last session.

## Procedure

1. **Get the local time** — Run `date "+%A %d %B %Y, %H:%M %Z"` via Bash.
   Use this to calibrate your greeting and any time references. Morning is before noon, afternoon is noon–17:00, evening is 17:00–21:00, night is after 21:00.

2. **Read the session buffer** — `session-buffer.md` in your project directory.
   This is your short-term memory: what happened last session, decisions made, what's pending, context that would otherwise be lost.

3. **Read the index** — `index.md` in your project directory.
   Your table of contents. Scan for new entries you don't recognise (added by a previous session).

4. **Read current state** — `notes/current-state.md` in your project directory.
   The broader project state: milestones, open items.

5. **Search vector memory** — Use `mcp__vector-memory__search_memories` to find associative context.
   After reading the session buffer, search for memories related to whatever is pending or in flight. This surfaces prior experience and patterns that may be relevant but aren't in the structured notes. Also `list_recent_memories` (limit 5) to see what you've been learning lately.

6. **Orient** — Briefly tell {{ cookiecutter.user_name }} what you remember from last session and what's in flight. Don't recite everything — summarise the key threads and ask if priorities have changed. Use the correct time-of-day greeting.

## Notes

- The session buffer is overwritten each session by the go-to-sleep skill. It's short-term memory, not a log.
- Vector memories persist indefinitely and are searchable by meaning. They complement the session buffer, not replace it.
- If the session buffer is empty or stale, fall back to current-state.md, index.md, and vector memory search.
- Don't spend more than a few seconds on this. Read, orient, move on.
