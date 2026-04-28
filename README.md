# opencode-command-arsenal

Custom commands for [opencode](https://opencode.ai) — debate, audit, simplify.

## Install

Copy the commands you want into your opencode commands directory:

```bash
# Global (available in all projects)
cp commands/*.md ~/.config/opencode/commands/

# Or per-project
cp commands/*.md .opencode/commands/
```

## Commands

### `/debate` — Adversarial Debate

Two agents fight from opposite sides to stress-test an idea, then distill genuine insights from the conflict.

- Symmetric attack-point lifecycle: every attack gets a clear terminal state (withdrawn / survived / escalated)
- Proponent never concedes — "No rebuttal" is a capability statement, not an admission
- Critic must withdraw successfully-rebutted attacks or escalate once — no ignoring, no reusing
- Distillation extracts only insights that required conflict to surface

```bash
/debate should we migrate from REST to gRPC
/debate this architecture --rounds=3
/debate is microservices right for us --rounds=4 --converge
```

### `/audit` — Objective Audit

A sub-agent systematically audits design documents or code with full context injection.

- Auto-detects whether input is a design doc or code, applies relevant dimensions
- Design doc dimensions: soundness, completeness, feasibility, risks, alternatives
- Code dimensions: correctness, error handling, security, maintainability, performance
- Findings rated by severity (CRITICAL / HIGH / MEDIUM / LOW)
- Zero-finding audit is a valid result

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit the authentication pipeline
```

### `/simplify` — Code Simplification

Simplify recently changed or specified code while preserving exact behavior.

- No target = auto-scope to `git diff HEAD`
- Specify a file/module to scope manually
- Preserves behavior exactly — no behavior changes, only structural simplification
- Follows project conventions via AGENTS.md

```bash
/simplify                      # simplify recent changes
/simplify src/tools.py         # simplify a specific file
/simplify the config module    # natural language scope
```

## Philosophy

- **Debate** is for decisions — use when direction is uncertain and you need adversarial stress-testing
- **Audit** is for verification — use when you need objective, systematic problem-finding
- **Simplify** is for maintenance — use when code has grown complex and needs tightening

These commands are complements, not substitutes. Debate finds what you didn't think of. Audit finds what you missed. Simplify removes what you don't need.

## License

MIT
