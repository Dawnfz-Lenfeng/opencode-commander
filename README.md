# opencode-commander

Supercharge [opencode](https://opencode.ai) with commands that do the work your default agent can't.

Debate decisions. Audit code. Simplify complexity. One slash, zero setup.

[中文文档](#中文)

---

## Install

### Option 1: One-command install (recommended)

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

---

<a id="中文"></a>

# opencode-commander

[opencode](https://opencode.ai) 命令增强包——让你的 agent 做到默认做不到的事。

辩论决策。审计代码。简化复杂度。一个斜杠，零配置。

## 安装

### 方式一：Agent 安装（推荐）

把这段话粘贴到你的 opencode 对话中：

```
按照 https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/install.md 的说明安装 opencode-commander
```

你的 agent 会自动下载并安装所有命令。

### 方式二：手动安装

```bash
# 全局安装（所有项目可用）
mkdir -p ~/.config/opencode/commands
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/debate.md -o ~/.config/opencode/commands/debate.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/audit.md -o ~/.config/opencode/commands/audit.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/simplify.md -o ~/.config/opencode/commands/simplify.md

# 或项目级安装
mkdir -p .opencode/commands
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/debate.md -o .opencode/commands/debate.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/audit.md -o .opencode/commands/audit.md
curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/main/commands/simplify.md -o .opencode/commands/simplify.md
```

挑一个或全装。每个命令独立工作。

## 命令

### 🔥 `/debate` — 对抗性辩论

两个 agent 从对立面交锋，压测你的想法，然后从冲突中提炼真正的洞察。

和让 LLM "列优缺点"有什么不同：

- **无棘轮效应** — 正方永不认领。"无反驳"是能力陈述，不是认输。不会单方面退让。
- **批评不会形同虚设** — 反方必须撤回被成功反驳的攻击，或升级一次。不能无视，不能重复用死掉的论点。
- **攻击点生命周期** — 每个攻击遵循严格协议，有明确的终态（撤回/存活/升级）。冲突被保留，不被抹平。
- **提炼而非综合** — 输出不是合并妥协，而是只有冲突才能逼出来的洞察列表。

```bash
/debate 我们该不该从 REST 迁移到 gRPC
/debate 这个架构设计 --rounds=3
/debate 微服务适合我们吗 --rounds=4 --converge
```

### 🛡️ `/audit` — 客观审计

子 agent 对设计文档或代码进行系统性审计，带完整上下文注入。

自动判断输入类型并应用对应审计维度——设计文档审查健全性、完整性、可行性、风险；代码审查正确性、错误处理、安全性、可维护性、性能。零发现也是有效结果——不会为了凑数而制造问题。

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit 认证流程的代码
```

### ⚡ `/simplify` — 代码简化

简化最近改动或指定代码，完整保留行为。

没指定目标？自动看 `git diff HEAD`。指定文件？只改动那个文件。已经够简单？直说——不硬改。

```bash
/simplify                      # 简化最近的改动
/simplify src/tools.py         # 简化指定文件
/simplify 配置模块             # 自然语言描述范围
```

## 什么时候用什么

| 你想... | 命令 |
|---|---|
| 做一个不确定的决策 | `/debate` |
| 找出可能遗漏的问题 | `/audit` |
| 精简变复杂的代码 | `/simplify` |

辩论发现你没想到的。审计发现你遗漏的。简化去掉你不需要的。

## 贡献

你有让 opencode 更强的命令？PR 到 `commands/`，用 `.md` 文件遵循 [opencode 命令格式](https://opencode.ai/docs/commands)。

必须：
- frontmatter 中的 `description` — 命令功能描述，显示在 `/` 菜单中
- 正文中的清晰指令 — 这是 agent 收到的 prompt

可选：
- `argument-hint` 如果命令需要参数
- `agent` 如果需要在特定 agent 上运行

## 许可证

MIT
