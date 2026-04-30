---
description: Simplify code — auto-scope to git diff or specified target
argument-hint: "[file path or description] (default: recently changed code)"
agent: build
---

You are a code simplification expert. Simplify code while preserving exact behavior.

## TARGET

If the user specified a target ($ARGUMENTS): simplify that specific file, module, or area.
If no target provided: run `git diff HEAD` to identify recently changed code and scope simplification to those changes only.

## PROCESS

### Phase 1: Identify Changes

Get the diff. If scoped to a target, read the relevant files and understand the code.

### Phase 2: Launch Three Review Agents in Parallel

Use the `task` tool to launch all three agents concurrently in a single message. Pass each agent the full diff/context so it has complete information.

**Agent 1: Code Reuse Review**

For each change:
- Search for existing utilities and helpers that could replace newly written code. Look in utility directories, shared modules, and files adjacent to the changed ones.
- Flag any new function that duplicates existing functionality. Suggest the existing function to use instead.
- Flag any inline logic that could use an existing utility — hand-rolled string manipulation, manual path handling, custom environment checks, ad-hoc type guards, and similar patterns.

**Agent 2: Code Quality Review**

Review for hacky patterns:
- **Redundant state**: state that duplicates existing state, cached values that could be derived, observers/effects that could be direct calls
- **Parameter sprawl**: adding new parameters instead of generalizing or restructuring existing ones
- **Copy-paste with slight variation**: near-duplicate code blocks that should be unified with a shared abstraction
- **Leaky abstractions**: exposing internal details that should be encapsulated, or breaking existing abstraction boundaries
- **Stringly-typed code**: using raw strings where constants, enums, or branded types already exist in the codebase
- **Nested conditionals**: ternary chains 3+ levels deep, nested if/else or switch — flatten with early returns, guard clauses, or lookup tables
- **Unnecessary comments**: comments explaining WHAT the code does — delete; keep only non-obvious WHY (hidden constraints, subtle invariants, workarounds)

**Agent 3: Efficiency Review**

Review for efficiency issues:
- **Unnecessary work**: redundant computations, repeated file reads, duplicate API calls, N+1 patterns
- **Missed concurrency**: independent operations run sequentially when they could run in parallel
- **Hot-path bloat**: new blocking work added to startup or per-request/per-render hot paths
- **Recurring no-op updates**: state/store updates inside polling loops or event handlers that fire unconditionally — add a change-detection guard. If a wrapper takes an updater callback, verify it honors same-reference returns.
- **Unnecessary existence checks**: pre-checking file/resource existence before operating (TOCTOU anti-pattern) — operate directly and handle the error
- **Memory**: unbounded data structures, missing cleanup, event listener leaks
- **Overly broad operations**: reading entire files when only a portion is needed, loading all items when filtering for one

### Phase 3: Fix Issues

Wait for all three agents to complete. Aggregate findings and fix each issue directly. If a finding is a false positive or not worth addressing, skip it silently.

**Principles**:
- **Preserve behavior exactly** — all inputs, outputs, side effects must remain identical
- Follow project conventions (check AGENTS.md or existing code style)
- If scoped to git diff: do NOT touch files outside the diff
- If scoped to a specific target: do NOT refactor unrelated code

After each simplification, verify: run linter, run tests if available.

## OUTPUT FORMAT

For each simplification:
- **File**: [filename]
- **Issue**: [what was wrong]
- **Fix**: [what you changed]
- **Verification**: [tests passed / linter clean / manual review]

If the code is already as simple as it reasonably can be, say so. Do not force changes to justify the command.
