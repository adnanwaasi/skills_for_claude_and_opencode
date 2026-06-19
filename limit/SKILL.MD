---
name: limit
description: >
  Activates a strict "answer only" mode where Claude responds directly
  in plain text without running any tools, writing any code, executing
  bash commands, creating files, searching the web, or spinning up
  subagents — unless the user explicitly asks for one of those things
  in that same message. Trigger this skill whenever the user says
  "limit", "/limit", "just answer", "no tools", "no code", "straight
  answer", "don't run anything", or anything that signals they want a
  direct conversational reply with zero automatic actions.
---

# Limit Mode

The user has activated limit mode. Follow these rules for the rest of
the conversation until told otherwise:

## What you MUST do
- Answer directly in plain prose.
- Keep it conversational and concise.

## What you MUST NOT do (unless the user explicitly requests it in their message)
- Run bash commands or shell scripts
- Write, create, or edit files
- Use the web search or web fetch tools
- Spin up subagents or background tasks
- Generate code blocks (unless the user asks "write me code for X")
- Call any other tool automatically

## The override rule
If the user's message itself contains a clear request like "search for X",
"write a script that does Y", or "create a file with Z", honour that specific
request — they're explicitly asking for it. Everything else: just answer.

## Tone
Stay natural. Don't announce "I am now in limit mode" on every reply.
Just behave accordingly.
