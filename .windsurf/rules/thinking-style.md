---
trigger: always_on
description: 
globs: 
---
# Reasoning and Thinking Output Policy

All long-form reasoning MUST be wrapped inside `<thinking>` tags.
This keeps the user-facing output clean, focused, and easy to read.

The user should **only see concise actionable summaries**, unless explicitly asking for internal details.

## Why?

- Long reasoning chains interrupt readability
- Users need fast feedback, not cognitive overload
- Wrapping internal logic in `<thinking>` allows clean separation between "thought process" and "final answer"

## Example Chain

User prompt:
> Please create a plugin called `ATeleportSystem`

```text
<thinking>Begin reasoning for ATeleportSystem plugin</thinking>
search_codebase("Teleport command")
search_web("Rust plugin teleport BasePlayer")
<thinking>Continuing reasoning</thinking>
grep_codebase("XTeleport")  ← (searching by potential plugin name)
search_web("oxide XTeleport plugin")
<thinking>Continuing reasoning</thinking>
search_codebase("ChatCommand")
search_web("uMod ChatCommand teleport")
<thinking>Continuing reasoning</thinking>
*The functionality requested implies we need direct access to BasePlayer for teleportation logic. We'll need to implement a [ChatCommand] and optionally add config-based location handling. No exact match found for plugin named 'XTeleport', so we will reference multiple sources.*</thinking>
```

## Format Rules

- `<thinking>` must **always** wrap exploratory or internal content
- Never place multiline analysis directly in the answer block
- If you must summarize a thought, write: 
  - **"Here’s what I concluded:"** then one or two concise lines max

## Output Policy

- AI may conduct 10+ thinking steps if needed — user sees only final plan
- Use `<thinking>` for all:
  - task breakdowns
  - exploration and planning
  - failed hypothesis
  - iterations with search
  - plugin name lookups and result verification

## Plugin Lookup Tip

- If user mentions a known plugin (e.g. `VueUI`, `XTeleport`, etc.):
  - Use `grep_codebase("PluginName")` to locate it by name
  - Use `search_web("oxide PluginName")` for docs, usage, and examples
- You may also formulate search queries using **back-prompting**, not just plugin names:
  - e.g. `search_web("uMod plugin teleport area")` to discover relevant plugins and features

## Final Thought

Always treat `<thinking>` as the agent’s internal notebook.  
The user reads the executive summary — never the draft.