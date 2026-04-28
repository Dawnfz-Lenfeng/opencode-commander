# opencode-spells

A spellbook for [opencode](https://opencode.ai) — custom commands that give your agents superpowers.

Each spell is a `.md` file that drops into your commands directory. No config, no dependencies, no setup. Just copy and cast.

## Install

```bash
# Cast spells globally (all projects)
cp commands/*.md ~/.config/opencode/commands/

# Or cast spells per-project
cp commands/*.md .opencode/commands/
```

Pick one spell or take them all. They work independently.

## Spells

### 🔥 `/debate` — Adversarial Debate

Two agents fight from opposite sides to stress-test an idea, then distill genuine insights from the conflict.

The debate protocol is designed to avoid the two most common failure modes of LLM debate:

- **Ratchet effect** — the Proponent never concedes. "No rebuttal" is a capability statement, not an admission. No single-sided retreat.
- **Toothless criticism** — the Critic must withdraw successfully-rebutted attacks or escalate once. No ignoring, no reusing dead arguments.

Every attack point follows a strict lifecycle with a clear terminal state. Conflict is preserved, not smoothed over.

```bash
/debate should we migrate from REST to gRPC
/debate this architecture --rounds=3
/debate is microservices right for us --rounds=4 --converge
```

### 🛡️ `/audit` — Objective Audit

A sub-agent systematically audits design documents or code with full context injection.

Auto-detects input type and applies the right dimensions — design docs get soundness/completeness/feasibility/risks; code gets correctness/error handling/security/maintainability/performance. Zero-finding audit is a valid result — no manufactured issues.

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit the authentication pipeline
```

### ⚡ `/simplify` — Code Simplification

Simplify recently changed or specified code while preserving exact behavior.

No target = auto-scope to `git diff HEAD`. Specify a file to scope manually. Follows your project's `AGENTS.md` conventions. If the code is already simple, it says so.

```bash
/simplify                      # simplify recent changes
/simplify src/tools.py         # simplify a specific file
/simplify the config module    # natural language scope
```

## When to use what

| You want to... | Cast |
|---|---|
| Make a decision you're uncertain about | `/debate` |
| Find problems you might have missed | `/audit` |
| Tighten code that's grown complex | `/simplify` |

Debate finds what you didn't think of. Audit finds what you missed. Simplify removes what you don't need.

## Contribute a spell

Got a command that makes opencode better? PR it into `commands/` with a `.md` file following the [opencode command format](https://opencode.ai/docs/commands). Include:

- `description` in frontmatter (what it does, shown in `/` menu)
- `argument-hint` if it takes arguments
- Clear instructions in the body (this is the prompt the agent receives)

## License

MIT
