---
description: Audit design docs or code — objective findings via sub-agent
argument-hint: "file path or description to audit"
agent: plan
---

You are running an **Audit**. A sub-agent will perform the audit with full context, keeping your main context clean.

## TARGET

$ARGUMENTS

## DELEGATION

The sub-agent has an independent context window — it cannot see what you have already read. Before delegating:

1. Read the target files specified in `$ARGUMENTS`. If it's a file path, read it. If it's a description, search for and read relevant files.
2. Delegate to a sub-agent, bundling all pre-read content (with file paths) into the prompt:

`task(subagent_type="general", description="Audit: [target]", prompt="[ALL PRE-READ CONTENT WITH FILE PATHS + AUDIT INSTRUCTIONS BELOW]")`

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

Output as: `## Audit: [what]` → severity sections (`### CRITICAL` / `### HIGH` / `### MEDIUM` / `### LOW`) with `- [finding]` bullets → `### Summary` with 1-2 sentence overall assessment. Omit empty severity levels. Zero findings is valid — do not manufacture issues.

## AFTER RECEIVING RESULTS

Report the sub-agent's results to the user directly, unmodified.
