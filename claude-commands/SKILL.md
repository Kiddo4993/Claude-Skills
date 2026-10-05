---
name: claude-commands
description: Reference of Claude Code's built-in special commands, CLI flags, and shortcuts — sessions (resume, rename, rewind, clear, compact, export), parallelism (background, fork, branch, batch, subtask, worktrees), context, modes, remote/desktop handoff. Use when the user asks "how do I ___ in Claude Code", "is there a command for ___", "what are the slash commands", or asks you to do something that is really a built-in command (rename this session, rewind, go to plan mode, background this, compact, etc.).
---

# claude-commands

Built-in slash commands are typed by the user; a skill cannot execute them. When a request maps to one, **do it with a tool if a tool equivalent exists** (column "Claude can do it"), otherwise hand the user the exact command to type. Verified against code.claude.com/docs, Sept 2026 — if a command errors, check `/help` since names change between versions.

## Sessions

| Goal | Type this | Claude can do it? |
|---|---|---|
| Resume last conversation | `claude --continue` (`-c`) | no |
| Pick a past conversation | `/resume` or `claude --resume [name]` | no |
| Name this session | `/rename <name>` · at start `claude -n <name>` | no |
| Branch into a copy | `/branch [name]` | no (see `fork-session`) |
| Background copy | `/fork [prompt]` | yes — `Agent` `subagent_type:"fork"` |
| Undo code + chat to a checkpoint | `/rewind` or `Esc Esc` | no |
| Fresh context | `/clear [name]` | no |
| Summarize to free context | `/compact [focus]` | no |
| See context usage | `/context` | no |
| Save transcript | `/export [file]` | no |
| Find the session that made a PR | `claude --from-pr <n>` | no |
| Move session to another dir | `/cd <path>` · add a dir: `/add-dir <path>` | no |

## Parallel & background

| Goal | Type this | Claude can do it? |
|---|---|---|
| Isolated branch checkout | `claude -w <name>` | yes — `EnterWorktree` (see `worktree`) |
| Detach this session to run on its own | `/background [prompt]` | no |
| Watch all background sessions | `claude agents` · `/tasks` | partly — `ListAgents` |
| Hand a side task to a helper | `/subtask <prompt>` | yes — `Agent` tool |
| Big change split across many agents | `/batch <instruction>` | no |
| Ask a side question without polluting context | `/btw <question>` | no |
| Repeat a prompt on an interval | `/loop [5m] <prompt>` | yes — `loop` skill |
| Cloud agent on a cron | `/schedule` | yes — `schedule` skill |
| Keep working toward a goal across turns | `/goal <condition>` | no |

## Modes & model

| Goal | Type this |
|---|---|
| Plan before editing | `Shift+Tab` until "plan mode", or `/plan`, or `claude --permission-mode plan` (Claude can use `EnterPlanMode`) |
| Change model / effort / fast | `/model`, `/effort <low…max>`, `/fast` |
| Permissions & hooks | `/permissions`, `/hooks`, `/fewer-permission-prompts` |
| Settings | `/config`, `/keybindings`, `/theme` |

## Handoff & devices

| Goal | Type this |
|---|---|
| Continue on phone/browser | `/remote-control` |
| Open in desktop app | `/desktop` |
| Pull a cloud session here | `/teleport` |

## Diagnostics

`/status`, `/usage`, `/doctor`, `/insights` (HTML report of your sessions), `/debug`, `/help`.
