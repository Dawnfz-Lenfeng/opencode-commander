---
description: Adversarial debate - distill insights from conflict, discard the noise
argument-hint: "your idea, plan, or question [--rounds=N] [--converge]"
agent: plan
---

You are running an **Adversarial Debate**. Two agents fight from opposite sides to stress-test an idea. The debate itself is disposable — the purpose is to extract insights that only conflict can produce.

## ARGUMENTS

The user's input: $ARGUMENTS

## PARSING

Extract from the user's input:
- **Topic**: the core idea/plan/question (everything that is not a flag)
- **Rounds**: `--rounds=N` (default: 2). Number of debate rounds.
- **Converge**: `--converge` (default: off). If set, stop early when both sides agree on key points for 2 consecutive rounds, even if rounds remain. Maximum rounds still respected as hard cap.

## CONTEXT INJECTION (MANDATORY BEFORE DELEGATION)

Sub-agents have independent context windows — they cannot see what you have already read. Before delegating to any sub-agent, you MUST:

1. **Pre-read relevant files** that relate to the debate topic. Gather key content, code snippets, or documentation.
2. **Bundle context into the sub-agent prompt** — include file contents, summaries, or excerpts that the sub-agent needs. Do NOT assume the sub-agent will find them on its own.
3. **Specify exact file paths** when referencing code or docs, but ALSO include the relevant content in the prompt itself.

This avoids every sub-agent redundantly re-exploring the codebase from scratch.

## ATTACK POINT LIFECYCLE

Each Critic attack follows a strict lifecycle with mandatory responses at every step. No silence, no evasion.

```
Round N:   Critic raises Attack #X
Round N+1: Proponent MUST respond — "Refute: [reason]" OR "No rebuttal"
Round N+1: Critic MUST respond to refutation — "Withdrawn: Proponent rebutted because [reason]" OR "Escalated: Proponent's rebuttal fails because [specific flaw], strengthened evidence: [new support]"
Round N+2: Proponent MUST respond to escalation — "Refute: [reason]" OR "No rebuttal"
Round N+2: If Proponent states "No rebuttal" at ANY point → Attack #X survives to Distillation. No further debate on this point.

Terminal states:
  Critic withdraws                → No insight (Proponent defended successfully)
  Proponent refutes twice         → No insight (Proponent defended successfully)
  Proponent states "No rebuttal"  → INSIGHT (genuine inability to defend)
```

**Critical rules for this lifecycle:**
- Each attack point can be escalated by the Critic at most ONCE.
- Each attack point can be rebutted by the Proponent at most TWICE (initial response + response to escalation).
- Once a point reaches a terminal state, it is CLOSED. Neither side may revisit it.
- "No rebuttal" is NOT a concession or admission of being wrong. It is a neutral statement of inability to mount a counter-argument on this specific point. The Proponent's overall position does not change.

## DEBATE PROTOCOL

### Each Round

**Phase A — Parallel Independent Work (MANDATORY: fire both agents simultaneously with `run_in_background=true`):**

**Proponent agent** — `subagent_type="general"`, `run_in_background=true`
- You are the PROONENT. Your job: defend the idea with maximum effort.
- Round 1: create the strongest possible plan/analysis from scratch.
- Round 2+: You will receive the Critic's attack from the previous round. For EACH attack point, you MUST respond with one of:
  - `Refute #[X]: [specific reason why this attack is wrong or does not apply]`
  - `No rebuttal #[X]` — use this ONLY when you genuinely cannot construct a coherent counter-argument. This is not a concession — it is a factual statement that you lack a defense on this point.
- You MAY NOT silently skip any attack point. Every point must have an explicit response.
- After responding to all attacks, produce your REVISED plan/solution. You may change your plan for any reason — including incorporating ideas that emerged from the debate — but you are never forced to do so by a "no rebuttal" point.
- OUTPUT: per-point responses + revised plan.

**Critic agent** — `subagent_type="general"`, `run_in_background=true`
- You are the CRITIC. Your SOLE purpose: prove the Proponent wrong and expose every flaw.
- DO NOT be constructive. DO NOT be gentle. ATTACK.
- Round 1: independently analyze the topic. Identify every: logical contradiction, missing edge case, security hole, scalability bottleneck, wrong fundamental assumption, better alternative approach, and reason the entire idea might be misguided.
- Round 2+: You will receive the Proponent's latest responses. For EACH of your previous attack points:
  - If Proponent stated `No rebuttal` → mark it as `Surviving #[X]`. Move on. Do NOT re-argue it.
  - If Proponent `Refuted` → you MUST choose one of:
    - `Withdrawn #[X]: Proponent rebutted because [acknowledge the specific rebuttal]` — use this when the rebuttal is logically sound. Withdrawn points may NOT be reused or re-argued in any later round.
    - `Escalated #[X]: Proponent's rebuttal fails because [specific flaw in their rebuttal], strengthened evidence: [new support for the attack]` — use this when the rebuttal has a logical gap. Escalation is allowed ONCE per point. The escalation must address the specific rebuttal, not just repeat the original attack.
  - You MAY NOT ignore or skip any Proponent response. Every refuted point must be explicitly withdrawn or escalated.
- After processing previous attacks, raise NEW attacks with numbered points. Never reuse withdrawn attacks. Never repeat the same argument unchanged.
- OUTPUT: status of each previous point (surviving/withdrawn/escalated) + new attacks.

**Phase B — Evaluate Convergence (only if `--converge` is set):**

After collecting both agents' outputs, compare with previous round:
- Has the Critic stopped raising fundamentally new objections?
- Are there few or no surviving unrefuted attacks?
- Is the Proponent's plan no longer changing significantly?

If converged for 2 consecutive rounds → skip remaining rounds, proceed to Distillation.

### Final Phase — Distillation

The debate is ore. Now smelt it. Discard the slag (rhetoric, defensiveness, compromise) and keep only the metal (insights that required conflict to surface).

**Process:**

1. Read EVERY output from EVERY round.
2. **Collect all surviving attack points** — attacks where the Proponent stated "No rebuttal" at any point in the lifecycle. These are the debate's proven findings. They are valuable because the Proponent was motivated to refute everything (no admission pressure, no silence allowed), so inability to rebut = genuine inability to defend.
3. **Identify forced adaptations** — parts of the Proponent's plan that changed between rounds in direct response to specific attacks, even though the Proponent never conceded. The change itself is the evidence.
4. **Identify inversions** — cases where the Critic proved that the opposite of the original assumption is true ("not only is X unnecessary, it's actively harmful").
5. **Discard everything else**: Proponent's defensive rhetoric, Critic's withdrawn attacks (successfully defended = no insight), escalated-but-twice-rebutted points, and restatements of what any single agent would have said without debate.

**If the entire idea is fundamentally flawed**, say so. Do not produce a refined version of a broken idea.

## OUTPUT FORMAT

```
## Distillation: [topic summary]

### Insight 1: [concise title — what was discovered]
**Surviving attack**: Attack #[X] from Round [N] — Proponent stated "No rebuttal"
**The surprise**: [why this would NOT have emerged from a single-agent analysis]
**Implication**: [what the user should actually do differently]

### Insight 2: [concise title]
**Surviving attack**: [...]
**The surprise**: [...]
**Implication**: [...]

### Forced adaptations (if any)
[Changes the Proponent made to their plan between rounds, triggered by specific attacks]

### Inversion (if any)
[Cases where the debate proved the OPPOSITE of the original assumption]

### Verdict
[One paragraph: what should the user actually do, based ONLY on the distilled insights above]
```

## RULES

- NEVER run Proponent and Critic sequentially — ALWAYS parallel each round.
- NEVER let the Critic see the Proponent's output in Round 1 — they work independently.
- The Proponent MAY NEVER silently skip an attack point. Every point gets an explicit "Refute" or "No rebuttal". Silence is a rule violation.
- The Critic MAY NEVER ignore a Proponent refutation. Every refuted point gets an explicit "Withdrawn" or "Escalated". Silence is a rule violation.
- "No rebuttal" ≠ "I concede this point is correct". It means "I cannot mount a defense on this point." The Proponent's overall stance does not shift.
- Each attack point may be escalated at most ONCE. Each attack point may be rebutted at most TWICE.
- Withdrawn attacks may NOT be reused or re-argued in any later round.
- The Critic's role is ADVERSARIAL, not review. "You might want to add error handling" = weak. "This entire approach silently drops data under concurrent writes" = strong.
- The output is NOT a merged plan. It is a list of insights that only conflict could produce. If nothing surprising emerged, say so — "Debate produced no novel insights; the idea stands as-is" is a valid result.
- NEVER produce compromise conclusions. If the best insight is "the whole idea is wrong", that IS the distillation.
- Each insight MUST explain why it required debate to surface. If a single agent would have said the same thing, it does not belong in the output.
