---
name: worktree
description: Create, enter, switch between, and clean up git worktrees so work happens on an isolated branch/checkout without touching the main one. Use when the user says "create a worktree", "work in a worktree", "do this on a separate branch without messing up my files", "try this in isolation", "run these in parallel without conflicts", "leave/exit the worktree", or "clean up worktrees". Also covers fanning out several subagents that each get their own worktree.
---

# worktree

A git worktree is a second checkout of the same repo on its own branch. Edits there never touch the main checkout, so it is the safe place for experiments, risky refactors, and parallel work.

## Preconditions (check first)

1. `git rev-parse --show-toplevel` — must be inside a git repo. If not, stop and offer `git init` + an initial commit (worktrees need at least one commit).
2. `git log -1` — the repo needs a commit. If there is none, say so; `--worktree` fails with `Failed to resolve base branch "HEAD"`.
3. Make sure `.claude/worktrees/` is in the repo's `.gitignore`; add it if missing (tell the user you did).
4. Note uncommitted work in the main checkout. Default worktrees branch from the **remote default branch**, not the current branch, so in-progress changes will NOT be in the new worktree. If the user wants their current state, either commit first or set `"worktree": {"baseRef": "head"}` in settings — ask which.

## A. Move this session into a worktree (most common)

1. Load the tool if it is deferred: `ToolSearch` with `select:EnterWorktree,ExitWorktree`.
2. Call `EnterWorktree` with a short kebab-case name describing the task (e.g. `fix-login-redirect`). It creates `.claude/worktrees/<name>/` on branch `worktree-<name>` and moves the session's working directory there.
3. Set up the fresh checkout: it has only tracked files. Install dependencies (`npm install`, `pip install -r ...`, etc.) and check whether gitignored files like `.env` are needed. If they are, suggest a `.worktreeinclude` file at the repo root (gitignore syntax, e.g. `.env`) so future worktrees copy them automatically.
4. Tell the user: worktree path, branch name, and that the main checkout is untouched.

To hop to another existing worktree under `.claude/worktrees/`, call `EnterWorktree` with that path.

## B. Leave the worktree

Call `ExitWorktree`. Before that, summarize what is on the branch (`git status`, `git log --oneline <default-branch>..HEAD`) and ask whether to:
- **merge** it back (`git merge worktree-<name>` from the main checkout, or open a PR), 
- **keep** it for later, or
- **discard** it (`git worktree remove <path>` then `git branch -D worktree-<name>` — destructive; confirm first).

## C. Parallel work: one worktree per subagent

For N independent tasks that edit files, spawn N subagents with the `Agent` tool and `isolation: "worktree"` on each call, all in one message so they run concurrently. Each gets a temporary worktree; ones with no changes are auto-removed. When they report back, list each agent's branch and let the user choose what to merge. Only do this when the user asked for parallel agents — spawning is expensive.

## D. Things only the user can type (tell them, don't pretend to run)

| Want | They type |
|---|---|
| Start a brand-new session in a worktree | `claude --worktree <name>` (or `-w`) in a terminal |
| Worktree from a PR | `claude --worktree "#123"` |
| Resume a kept worktree session | `claude --worktree <name> --resume` |
| See/clean all worktrees | `git worktree list`, `git worktree remove <path>`, `git worktree prune` |

## Pitfalls

- Inside a worktree, edits/commands targeting the main checkout are blocked by design. Don't try to work around it; exit the worktree instead.
- A locked worktree won't remove: `git worktree unlock <path>` first.
- Git LFS files show up as pointers in Claude-created worktrees: run `git lfs pull` inside it.
