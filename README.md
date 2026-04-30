# opencode-commander

Supercharge [opencode](https://opencode.ai) with commands that do the work your default agent can't.

**[English](#english)** | **[中文](README.zh-CN.md)**

---

<a id="english"></a>

## Why

opencode is great. But you keep running into the same problems:

- You ask "is this approach good?" and the agent says "looks good, a few suggestions..." It will never say "this direction is fundamentally wrong." That kind of honesty only comes from adversarial pressure.
- You delegate to a sub-agent and it starts from scratch — re-reading your entire codebase. Every time. Your context budget bleeds.
- "Simplify this code" — simplify *what*? Which file? The agent doesn't know the scope, and you don't know how to define it.
- You want to enhance your opencode workflow, but you don't want a plugin framework that takes over everything. You just want a few precise commands — drop in, use, done.

**opencode-commander's philosophy: lightweight, precise, no takeover.**

Each command solves one specific pain point. No changes to your default agent. No dependencies. No configuration. Copy a `.md` file into your commands directory, restart, done.

## Install

Paste this into your opencode session:

```
Install opencode-commander by following the instructions at https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/docs/install.md
```

Your agent will clone the repo and symlink commands automatically.

## Update

```bash
cd ~/.opencode-commander && git pull
```

Symlinks update automatically.

## Commands

### 🔥 `/debate` — Adversarial Debate

Two agents fight from opposite sides, then distill insights from the conflict — not a merged compromise.

```bash
/debate should we migrate from REST to gRPC
/debate this architecture --rounds=3
/debate is microservices right for us --rounds=4 --converge
```

### 🛡️ `/audit` — Objective Audit

A sub-agent systematically audits design docs or code with full context injection — no re-reading the codebase from scratch.

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit the authentication pipeline
```

### ⚡ `/simplify` — Code Simplification

Simplify code while preserving exact behavior. No target? Auto-scopes to `git diff HEAD`.

```bash
/simplify                      # recent changes
/simplify src/tools.py         # specific file
/simplify the config module    # natural language scope
```

## When to use what

| You want to... | Command |
|---|---|
| Make a decision you're uncertain about | `/debate` |
| Find problems you might have missed | `/audit` |
| Tighten code that's grown complex | `/simplify` |

## Contribute

Got a command that makes opencode better? PR it into `commands/` with a `.md` file following the [opencode command format](https://opencode.ai/docs/commands).

Required: `description` in frontmatter + clear instructions in the body.
Optional: `argument-hint`, `agent`, `subtask`.

## License

MIT
