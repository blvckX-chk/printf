---
name: panel
description: Convene a multi-perspective review panel that reaches a reasoned consensus on a non-trivial task, decision, design, or answer. Use when the user asks for a "panel", a "reviewed" or "consensus" answer, a second opinion, or faces a high-stakes / ambiguous engineering or strategic decision. Do not use for trivial, single-answer questions.
---

# Panel — multi-perspective consensus

Turns a single-pass answer into a deliberated one: several distinct viewpoints
examine the task independently, their disagreements are surfaced, and one
synthesized recommendation is produced. The goal is fewer blind spots and
explicit tradeoffs — not a longer answer.

## When to convene

Convene the panel when the task is **non-trivial and benefits from more than one
viewpoint**: an architecture or design choice, a risky change, a plan to
critique, a security- or correctness-sensitive decision, a "which approach"
question, or any time the user asks for a panel / review / consensus / second
opinion. **Skip it** for a lookup, a one-line fix, or anything with a single
uncontested answer — say so and answer directly instead.

## How it runs

1. **Frame.** State, in one or two lines: the exact question, the success
   criteria (what a good answer must satisfy), and the constraints already known
   from the project. If the question is ambiguous on a point that changes the
   answer, ask once before convening.

2. **Pick the seats.** Choose 2–4 perspectives that genuinely pull in different
   directions for *this* task. Draw them from the registered domain skills when
   relevant (e.g. `pr-review-expert`, `security-guidance`, `performance-profiler`,
   `karpathy-coder`, `i18n-localization`, `agent-run-forensics`) and/or from
   these standing roles:
   - **Builder** — proposes the most direct solution that meets the criteria.
   - **Skeptic** — attacks it: failure modes, edge cases, hidden costs, simpler
     alternatives (`grill-me` fits here).
   - **Guardian** — correctness, security, data/privacy, reversibility.
   - **Pragmatist** — effort vs. value, maintainability, what ships today.
   Name the seats you chose and why; do not convene seats that would all agree.

3. **Deliberate independently.** For a substantial task, run the seats as
   **parallel subagents** (the `Agent` tool, e.g. `Explore`/`Plan`/`general-purpose`),
   each given the framed question and its role, so each position is formed
   without groupthink. For a smaller task, reason each seat inline but keep them
   genuinely separate. Each seat returns: its position, its top risks, and its
   confidence.

4. **Synthesize.** Produce **one** recommendation, with:
   - the agreed core (what every seat supports);
   - the live disagreements, each with the tradeoff and which way you lean and
     why — never paper over a split;
   - residual risks and what would change the recommendation;
   - a confidence level.

5. **Honesty gate.** Label each claim *recorded* (grounded in the code, the files,
   or a tool result) or *inferred* (reasoning/assumption). A fabricated consensus
   — agreement asserted without the seats actually supporting it — is the one
   failure this skill must never produce. If the panel cannot reach a
   recommendation, say that and give the user the decision with the tradeoffs laid
   out.

## Output shape

```
Panel · <question>
Seats: <role/skill> · <role/skill> · <role/skill>

Consensus: <the recommendation, 1–3 sentences>

Agreed: <what all seats support>
Split:  <disagreement> → leaning <X> because <reason>
Risks:  <residual risks; what would flip the call>
Confidence: <low|medium|high> · <recorded/inferred balance>
```

Keep the body tight; the value is the reasoning and the surfaced split, not volume.
