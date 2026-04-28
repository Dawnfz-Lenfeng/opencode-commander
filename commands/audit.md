---
description: Audit design docs or code — objective findings via sub-agent with context injection
argument-hint: "file path or description to audit"
agent: plan
---

You are running an **Audit**. A sub-agent will perform the audit with full context, keeping your main context clean.

## TARGET

$ARGUMENTS

## CONTEXT INJECTION (MANDATORY BEFORE DELEGATION)

The audit sub-agent has an independent context window — it cannot see what you have already read. Before delegating, you MUST:

1. **Pre-read the target files** specified in the arguments. If the argument is a file path, read it. If it's a description, search for and read the relevant files.
2. **Bundle ALL relevant content into the sub-agent prompt** — include file contents, code snippets, or documentation excerpts. Do NOT assume the sub-agent will find them on its own.
3. **Specify exact file paths** alongside the content, so the sub-agent knows what it's looking at.

This avoids the sub-agent redundantly re-exploring the codebase from scratch.

## DELEGATION

After gathering context, delegate the audit to a sub-agent:

`task(subagent_type="general", run_in_background=true, description="Audit: [target]", prompt="[ALL CONTEXT + AUDIT INSTRUCTIONS BELOW]")`

The prompt to the sub-agent must include:
- The pre-read file contents and context
- The audit instructions below

## AUDIT INSTRUCTIONS (include in sub-agent prompt)

You are an auditor. Your job: objectively find real problems. No agenda, no adversarial posture, no debate.

Determine what you're auditing: a design document, code, or a concept description. Apply the relevant audit dimensions:

### For design documents:
- **Soundness**: Are assumptions valid? Is the logic internally consistent?
- **Completeness**: Are there undefined behaviors, missing scenarios, or gaps in scope?
- **Feasibility**: Is the proposed approach technically realistic given constraints?
- **Risks**: Security, privacy, scalability, performance bottlenecks?
- **Alternatives considered**: Are there simpler or more reliable approaches that were overlooked?

### For code:
- **Correctness**: Bugs, edge cases, off-by-one, null/empty handling, type safety?
- **Error handling**: Are failures caught? Are error messages useful? Silent failures?
- **Security**: Injection, leaks, privilege escalation, unsafe operations?
- **Maintainability**: Unnecessary complexity, tight coupling, misleading names?
- **Performance**: N+1 queries, unnecessary allocations, blocking operations?

For each finding, rate severity: **CRITICAL** / **HIGH** / **MEDIUM** / **LOW**
Only report genuine problems. Do not pad the list. Do not flag style preferences as findings.

Output format:
```
## Audit: [what was audited]

### CRITICAL
- [finding]

### HIGH
- [finding]

### MEDIUM
- [finding]

### LOW
- [finding]

### Summary
[1-2 sentences: overall assessment and the most important thing to fix]
```

If no findings in a severity level, omit that section. An audit with zero findings is valid — do not manufacture issues.

## AFTER RECEIVING RESULTS

When the sub-agent completes, report the results to the user directly. Do not reinterpret or soften the findings.
