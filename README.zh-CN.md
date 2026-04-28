# opencode-commander

[opencode](https://opencode.ai) 命令增强包——让你的 agent 做到默认做不到的事。

辩论决策。审计代码。简化复杂度。一个斜杠，零配置。

**[中文](#中文)** | **[English](README.md)**

---

<a id="中文"></a>

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
