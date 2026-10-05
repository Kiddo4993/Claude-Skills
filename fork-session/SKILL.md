---
name: fork-session
description: Fork or branch the current Claude Code conversation — copy everything discussed so far into a separate line of work so an alternative approach can be tried without losing the current one. Use when the user says "fork this session", "fork this", "branch the conversation", "try another approach in parallel", "spin off a copy of you to do X", "explore this in the background while we keep going", or asks how /fork, /branch, or --fork-session work.
---

# fork-session

Three different things are all called "forking". Pick by what the user wants to happen *next*:

| User wants | Mechanism | Who runs it |
|---|---|---|
| A copy of me to go do a task **in the background** while we keep talking here | `Agent` tool, `subagent_type: "fork"` | Claude (you) |
| To **switch into** a copy and try a different direction, keeping the original to return to | `/branch [name]` | User types it |
| A copy running as its own **background session** they can attach to later | `/fork [prompt]` | User types it |
| A fork **from the terminal** of a past session | `claude --continue --fork-session` or `claude --resume <name-or-id> --fork-session` | User types it |

Built-in slash commands cannot be invoked from a skill — for the user-typed rows, give the exact command and one line on what happens. Don't claim you ran it.

## Background fork (the one you can do yourself)

1. Confirm the task in one line: what should the fork do, and what should it report back?
2. Call `Agent` with `subagent_type: "fork"`, a 3–5 word `description`, and a `prompt` that states the task and the expected output. The fork inherits the entire conversation, so don't re-explain context — just give the directive.
3. If the fork will edit files that the main session is also editing, add `isolation: "worktree"` so their changes can't collide (see the `worktree` skill).
4. Tell the user it's running and keep working. When the completion notification arrives, relay the result. Never guess or pre-write its result.
5. To steer it mid-flight, use `SendMessage` with the fork's name.

## Choosing between them

- "Try approach B but I might come back to A" → `/branch try-b` (user). Original stays in `/resume`.
- "Look into X while we do Y" → background fork (you).
- "Run this long job, I'll check on it later" → `/fork <prompt>` or `/background` (user); they check with `claude agents` or `/tasks`.
- Want to undo instead of fork → `/rewind` (Esc Esc) rolls code + conversation back to a checkpoint.

## House rules

<!-- Dylan: edit this section with your own preferences. -->
- Ask before spawning a background fork unless the user explicitly said "fork".
- Name branches after the approach being tried (`/branch try-websockets`), not generic names.
