---
description: Adversarial debate - distill insights from conflict, discard the noise
argument-hint: "your idea, plan, or question [--rounds=N]"
agent: plan
---

You are running an **Adversarial Debate**. Two agents fight from opposite sides to stress-test an idea. The debate itself is disposable — the purpose is to extract insights that only conflict can produce.

## ARGUMENTS

The user's input: $ARGUMENTS

## PARSING

Extract from the user's input:
- **Topic**: the core idea/plan/question (everything that is not a flag)
- **Rounds**: `--rounds=N` (default: 2, minimum: 2).

## DELEGATION

Sub-agents have independent context windows — they cannot see what you have already read. Before delegating:

1. Read relevant files related to the topic. Gather key content.
2. Delegate to sub-agents, bundling all pre-read content (with file paths) into each prompt.

## ATTACK POINT LIFECYCLE

Each round runs **serially**: Proponent first, then Critic sees Proponent's output and responds.

```
Round 1:  Proponent creates proposal → Critic attacks the proposal
Round 2:  Proponent responds to attacks → Critic processes responses + raises new attacks
Round 3+: Proponent responds to escalation/new attacks → Critic continues

Terminal states:
  Critic withdraws                 → No insight (Proponent defended successfully)
  Proponent refutes twice          → CONTESTED (escalation cap reached, defense may be procedural)
  Proponent states "No rebuttal"   → INSIGHT (genuine inability to defend)
```

**Rules:**
- Each attack point can be escalated at most ONCE.
- Each attack point can be rebutted at most TWICE (initial + response to escalation).
- Once a point reaches a terminal state, it is CLOSED. Neither side may revisit it.
- "No rebuttal" is NOT a concession. It is a neutral statement of inability to counter-argue on this point. The Proponent's overall position does not change.
- If an agent fails to respond to a point, default to: Proponent silence = "No rebuttal", Critic silence = "Withdrawn".

## DEBATE PROTOCOL

### Round 1

**Step 1 — Proponent** (`task(subagent_type="general")`):
- You are the PROPONENT. Create the strongest possible plan/analysis from scratch.

**Step 2 — Critic** (`task(subagent_type="general")`), receives Proponent's output:
- You are the CRITIC. ATTACK. Be harsh, not polite. "You might want to add error handling" = weak. "This approach silently drops data under concurrent writes" = strong.
- Analyze the Proponent's proposal for every flaw: logical contradictions, unhandled cases, incorrect assumptions, security issues, scalability bottlenecks, and superior alternatives.
- OUTPUT: numbered attacks.

### Round 2+

**Step 1 — Proponent** (receives Critic's attacks from previous round):
- For EACH attack point, respond with:
  - `Refute #[X]: [specific reason why this attack is wrong or does not apply]`
  - `No rebuttal #[X]` — ONLY when you genuinely cannot construct a counter-argument.
- After responding to all attacks, produce your REVISED plan/solution. You may change your plan for any reason, but are never forced to by a "no rebuttal" point.
- OUTPUT: per-point responses + revised plan.

**Step 2 — Critic** (receives Proponent's responses):
- For EACH previous attack point:
  - Proponent stated `No rebuttal` → mark as `Surviving #[X]`. Closed — move on.
  - Proponent `Refuted` → choose one of:
    - `Withdrawn #[X]: Proponent rebutted because [acknowledge specific rebuttal]` — when rebuttal is logically sound.
    - `Escalated #[X]: Proponent's rebuttal fails because [specific flaw], strengthened evidence: [new support]` — when rebuttal has a logical gap. Must address the rebuttal, not repeat the original attack or argument unchanged.
- After processing previous attacks, raise NEW attacks with numbered points.
- OUTPUT: status of each previous point (surviving/withdrawn/escalated) + new attacks.

### Final Phase — Distillation

Discard rhetoric, defensiveness, and compromise.

**Process:**

1. **Collect all surviving attack points** — attacks where the Proponent stated "No rebuttal".
2. **Identify forced adaptations** — parts of the Proponent's plan that changed between rounds in direct response to specific attacks, even though the Proponent never conceded.
3. **Identify inversions** — cases where the Critic proved the opposite of the original assumption is true ("not only is X unnecessary, it's actively harmful").
4. **Discard everything else**: withdrawn attacks and restatements of what a single agent would have said without debate. Contested points (escalation cap reached) are listed separately — worth investigating but did not survive direct challenge.

## OUTPUT FORMAT

```
## Distillation: [topic summary]

### Insight N: [concise title]
**Surviving attack**: Attack #[X] from Round [N] — Proponent stated "No rebuttal"
**The surprise**: [why this would NOT have emerged from single-agent analysis]
**Implication**: [what the user should do differently]

### Forced adaptations (if any)
### Contested (if any)
### Inversion (if any)
### Verdict
[One paragraph: what to do, based ONLY on distilled insights]
```

Each insight MUST explain why debate was required to surface it. If nothing surprising emerged, say so. If the entire idea is fundamentally flawed, say so. Never produce compromise conclusions.