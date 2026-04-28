# opencode-commander

[opencode](https://opencode.ai) 命令增强包——让你的 agent 做到默认做不到的事。

**[中文](#中文)** | **[English](README.md)**

---

<a id="中文"></a>

## 为什么需要这个

opencode 很好用。但你会反复遇到同样的问题：

- 你问"这个方案行不行"，agent 说"看起来不错，有几点建议……"它永远不会说"这个方向根本就是错的"。这种诚实，只有对抗性压力才能逼出来。
- 你派子 agent 干活，它从零开始——重新读你的整个代码库。每次都重新读。你的上下文预算就这么浪费了。
- "帮我简化一下代码"——简化什么？哪个文件？agent 不知道范围，你也不知道怎么界定。
- 你想增强你的 opencode 工作流，但不想装一个接管一切的插件框架。你只想要几个精准的命令——放进目录就能用。

**opencode-commander 的哲学：轻量、精准、不接管。**

每个命令解决一个具体的痛点。不改变你的默认 agent，不添加依赖，不需要配置。复制 `.md` 文件到 commands 目录，重启，生效。

## 安装

把这段话粘贴到你的 opencode 对话中：

```
按照 https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/docs/install.md 的说明安装 opencode-commander
```

你的 agent 会自动下载并安装所有命令。

## 命令

### 🔥 `/debate` — 对抗性辩论

两个 agent 从对立面交锋，然后从冲突中提炼洞察——不是合并妥协。

```bash
/debate 我们该不该从 REST 迁移到 gRPC
/debate 这个架构设计 --rounds=3
/debate 微服务适合我们吗 --rounds=4 --converge
```

### 🛡️ `/audit` — 客观审计

子 agent 对设计文档或代码进行系统性审计，带完整上下文注入——不用从零开始重新读代码库。

```bash
/audit src/tools.py
/audit docs/architecture.md
/audit 认证流程的代码
```

### ⚡ `/simplify` — 代码简化

简化代码，完整保留行为。没指定目标？自动看 `git diff HEAD`。

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

## 贡献

你有让 opencode 更强的命令？PR 到 `commands/`，用 `.md` 文件遵循 [opencode 命令格式](https://opencode.ai/docs/commands)。

必须：frontmatter 中的 `description` + 正文中的清晰指令。
可选：`argument-hint`、`agent`、`subtask`。

## 许可证

MIT
