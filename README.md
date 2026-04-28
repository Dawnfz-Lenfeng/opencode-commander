# opencode-commander

Supercharge [opencode](https://opencode.ai) with commands that do the work your default agent can't.

Debate decisions. Audit code. Simplify complexity. One slash, zero setup.

**[English](#english)** | **[中文](README.zh-CN.md)**

---

<a id="english"></a>

## Install

### Option 1: Agent install (recommended)

Paste this into your opencode session:

```
Install opencode-commander by following the instructions at https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/install.md
```

Your agent will download and install all commands automatically.

### Option 2: Manual install

```bash
# Global (all projects)
mkdir -p ~/.config/opencode/commands
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/debate.md -o ~/.config/opencode/commands/debate.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/audit.md -o ~/.config/opencode/commands/audit.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/simplify.md -o ~/.config/opencode/commands/simplify.md

# Or per-project
mkdir -p .opencode/commands
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/debate.md -o .opencode/commands/debate.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/audit.md -o .opencode/commands/audit.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/simplify.md -o .opencode/commands/simplify.md
```

Pick one or take them all. Each command works independently.

## Commands

### 🔥 `/debate` — Adversarial Debate

Two agents fight from opposite sides to stress-test an idea, then distill genuine insights from the conflict.

What makes this different from asking an LLM to "list pros and cons":

- **No ratchet effect** — the Proponent never concedes. "No rebuttal" is a capability statement, not an admission. No single-sided retreat.
- **No toothless criticism** — the Critic must withdraw successfully-rebutted attacks or escalate once. No ignoring, no reusing dead arguments.
- **Attack-point lifecycle** — every attack follows a strict protocol with a clear terminal state (withdrawn / survived / escalated). Conflict is preserved, not smoothed over.
- **Distillation, not synthesis** — the output isn't a merged compromise. It's a list of insights that only conflict could produce.

```bash
/debate should we migrate from REST to gRPC
/debate this architecture --rounds=3
/debate is microservices right for us --rounds=4 --converge
```

### 🛡️ `/audit` — Objective Audit

A sub-agent systematically audits design documents or code with full context injection.

Auto-detects input type and applies the right audit dimensions — design docs get soundness, completeness, feasibility, risks; code gets correctness, error handling, security, maintainability, performance. Zero-finding audit is a valid result — no manufactured issues.

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit the authentication pipeline
```

### ⚡ `/simplify` — Code Simplification

Simplify recently changed or specified code while preserving exact behavior.

No target specified? Auto-scopes to `git diff HEAD`. Point it at a file? Only that file. Already simple? It says so — no forced changes.

```bash
/simplify                      # simplify recent changes
/simplify src/tools.py         # simplify a specific file
/simplify the config module    # natural language scope
```

## When to use what

| You want to... | Command |
|---|---|
| Make a decision you're uncertain about | `/debate` |
| Find problems you might have missed | `/audit` |
| Tighten code that's grown complex | `/simplify` |

Debate finds what you didn't think of. Audit finds what you missed. Simplify removes what you don't need.

## Contribute

Got a command that makes opencode better? PR it into `commands/` with a `.md` file following the [opencode command format](https://opencode.ai/docs/commands).

Required:
- `description` in frontmatter — what it does, shown in the `/` menu
- Clear instructions in the body — this is the prompt the agent receives

Optional:
- `argument-hint` if it takes arguments
- `agent` if it should run on a specific agent

## License

MIT
