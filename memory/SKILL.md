---
name: opencode-memory
description: >
  Maintain a persistent memory file (AGENTS.md) for OpenCode and Claude Code projects — equivalent to CLAUDE.md. Use this skill whenever the user mentions OpenCode, AGENTS.md, project memory, session context, or wants to capture/update project knowledge so it persists across coding sessions. Trigger even for casual mentions like "remember this for next time", "update the project file", "save our decisions", "I use opencode", or "keep track of what we built". This skill creates, reads, and updates the AGENTS.md file with project structure, build/run commands, and a changelog of key decisions.
---

# OpenCode Memory Skill

Manages `AGENTS.md` — a persistent project memory file read automatically by OpenCode (and Claude Code) at the start of every session. It is the open-source equivalent of `CLAUDE.md`.

## When to use this skill

- User starts work in a new project that has no AGENTS.md yet → **create** one
- User asks to "remember", "save", "log", or "update" something about the project → **update** it
- User asks what the project does, what commands to run, or what decisions were made → **read** it first
- User finishes a meaningful change (new feature, refactor, architectural decision) → **append** to the changelog

---

## File location

Always place at the **project root**:
```
<project-root>/AGENTS.md
```

If the user is working in a monorepo or sub-package, also consider placing one at the sub-package root.

---

## AGENTS.md template

When creating from scratch, use this structure. Only include sections where you actually have content — don't leave empty placeholders.

```markdown
# Project Memory

> Auto-maintained by the opencode-memory skill. Edit freely.

## Project Overview
<!-- One paragraph: what this project does and its main tech stack -->

## Architecture
<!-- Key structural decisions: monorepo vs single package, framework choices, data flow -->

## Project Structure
<!-- Only the important parts — skip obvious dirs like node_modules -->
<project-root>/
├── src/          # ...
├── tests/        # ...
└── ...

## Commands
| Action        | Command              |
|---------------|----------------------|
| Install       | `npm install`        |
| Dev server    | `npm run dev`        |
| Build         | `npm run build`      |
| Test          | `npm test`           |
| Lint          | `npm run lint`       |

## Key Files
| File | Purpose |
|------|---------|
| `src/index.ts` | Entry point |

## Decisions & Changelog
<!-- Most recent first. Format: `YYYY-MM-DD — Decision or change` -->
- 2026-06-03 — Project initialized
```

---

## How to create AGENTS.md

1. **Scan the project** before writing — read `package.json`, `pyproject.toml`, `Cargo.toml`, `Makefile`, `README.md`, or whatever build config exists
2. **Discover structure** — list the top-level directories, identify the entry point, find test dirs
3. **Extract commands** — pull scripts from `package.json`, `Makefile` targets, or `pyproject.toml` `[tool.scripts]`
4. **Write the file** — populate only sections with real content; today's date for the first changelog entry
5. **Tell the user** where it was saved and what's in it

---

## How to update AGENTS.md

Read the existing file first, then make targeted edits:

- **New command discovered** → add a row to the Commands table
- **New key file added** → add a row to Key Files
- **Architectural decision made** → prepend to Decisions & Changelog (newest first)
- **Project structure changed** → update the tree
- **Old info is wrong** → fix it in place

Keep the file **concise** — it loads into every session. Aim for under 100 lines. If it grows beyond that, ruthlessly trim stale entries from the changelog (keep last 10–15) and merge redundant structure notes.

---

## Changelog entry format

```
- YYYY-MM-DD — <What changed or was decided, and why if non-obvious>
```

Examples:
```
- 2026-06-03 — Switched from Vite to Turbopack for faster HMR in dev
- 2026-06-02 — Added Zod validation layer before all DB writes
- 2026-06-01 — Initial project scaffolded with Next.js 15 + Tailwind
```

---

## Reading AGENTS.md for context

When the user asks a question that AGENTS.md would answer (commands, structure, past decisions), **read it first** before answering. This avoids re-scanning the whole repo and respects decisions already logged.

```bash
cat AGENTS.md
```

If the file doesn't exist yet, offer to create it.

---

## Notes

- **OpenCode** reads `AGENTS.md` natively at session start — no configuration needed
- **Claude Code** reads `CLAUDE.md` natively; if you're on Claude Code, either rename the file or create a `CLAUDE.md` that simply does `@AGENTS.md` to include it
- Both tools treat the file as **context, not enforced config** — the model reads it and uses good judgment
- Don't store secrets, API keys, or anything sensitive in this file
- If the project already has a `CLAUDE.md`, read it and migrate its content into `AGENTS.md` (or vice versa) rather than maintaining two files
