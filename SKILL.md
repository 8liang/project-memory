---
name: project-memory
description: Read and maintain durable shared project knowledge. Uses Fast Note Sync MCP (FNS) when configured, otherwise a docs/memory/ directory in the repo.
---

# Shared Project Memory

The canonical long-term project memory lives in one of two backends:

1. **FNS backend** — the project's knowledge base exposed through
   Fast Note Sync MCP (FNS), when those MCP tools are available.
2. **File backend** — `docs/memory/` at the project root, when no FNS
   MCP server is configured or reachable.

Pick the backend once per session, as described in
[Choosing a backend](#choosing-a-backend), and use it consistently.

## Choosing a backend

1. If MCP tools from an FNS server are available (search/list/read/write
   note tools), use the **FNS backend**.
2. Otherwise use the **file backend** at `docs/memory/`. Do not ask the
   user for permission to use it, and do not stop because FNS is missing.

Never mirror the same knowledge into both backends. If FNS is present,
`docs/memory/` is not used; if it is absent, FNS is not used.

## File backend layout

```
docs/memory/
├── MEMORY.md        # index: one line per topic file
├── architecture.md
└── payments-decisions.md
```

- `MEMORY.md` is the index loaded first. One line per topic file:
  `- [Title](file.md) — hook`.
- One file per topic, kebab-case filenames. Keep each file focused;
  split when a file grows to cover unrelated subjects.
- Create `docs/memory/` lazily, on the first durable fact worth
  persisting, and say in your reply that you created it.

## The index is not the memory (both backends)

Whichever backend is active, the memory index (`memory/MEMORY.md` in FNS,
`docs/memory/MEMORY.md` for files, or whatever index the project names)
is loaded into every session. Keep it small.

- One line per entry: `- [[target-note]] — short hook` (FNS) or
  `- [Title](file.md) — short hook` (files). The hook says what the note
  covers and when to read it, in roughly one short sentence.
- All content goes in the target note: details, evidence, commands,
  numbers, history, corrections. If no target note exists, create one
  first, then add its index line.
- When updating an existing memory, edit the target note. Touch the
  index line only if its hook is no longer accurate. Never append
  "update"/"correction" text to the index line.
- Never put in the index: dated changelog blocks ("> 2026-10-14: added
  ..."), multi-sentence or multi-paragraph entries, content that has no
  target note, or session/release logs.
- After writing, check that every index line you touched is still a
  single short line. If the index has grown large (more than ~15 KB),
  consolidate entries instead of adding more.
- If the project's own instructions (AGENTS.md, CLAUDE.md) give stricter
  index rules, follow those.

## Before working

When historical project context may affect the task:

1. Read the memory:
   - FNS: search the shared knowledge base.
   - Files: read `docs/memory/MEMORY.md`, then the topic files whose
     index lines look relevant.
2. Prefer existing project decisions over assumptions.
3. Do not treat agent-specific conversation history as canonical
   project knowledge.

## During work

When discovering durable knowledge, including:

- architecture decisions
- important implementation constraints
- non-obvious bugs
- conventions
- rejected approaches and reasons
- important module relationships

persist it to the active backend.

## Do not persist

Do not store:

- temporary debugging output
- trivial implementation details
- speculative ideas
- conversation summaries with no durable value
- information already documented elsewhere (point to it instead)

## Updating memory

Before creating a new entry:

1. Search for an existing relevant entry (FNS search, or grep
   `docs/memory/`).
2. Update it if one exists.
3. Create a new entry only when necessary.
4. Keep knowledge concise and factual.
5. After adding or renaming a topic note (either backend), update its
   single line in the index — see
   [The index is not the memory](#the-index-is-not-the-memory-both-backends).
