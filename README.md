# Agent Governance Skills for OpenClaw

**Make your agent's rules actually hold.** Five small skills that close the gap between *what you told the agent* and *what it does*.

Rule files drift. Prompts get skipped. Nobody notices until something breaks. This set treats agent governance as an engineering problem: measure it, block it, slim it, review it.

---

## The problem this solves

Most agent failures are not model failures — they are **governance failures**:

| Failure | What it looks like |
|---|---|
| **Rules exist but never fire** | A rule was written months ago and has not triggered once. It is decoration, not a control. |
| **Rules exist but cannot be checked** | Written as prose. No threshold, no gate, nothing to count. |
| **The rulebook outgrew the context window** | 20k+ characters of instructions, competing for attention with the actual task. |
| **Nobody verifies the verifier** | A checker that passes everything looks identical to a system with no problems. |

Each skill below addresses one of these.

---

## The five skills

| Skill | Question it answers | Skip it and... |
|---|---|---|
| **[agent-rule-enforcement](https://github.com/cengcengcai-creator/agent-rule-enforcement)** | Are my rules actually enforced, or only written down? | You cannot tell which rules are controls and which are decoration. |
| **[agent-instruction-slimming](https://github.com/cengcengcai-creator/agent-instruction-slimming)** | How do I shrink the rulebook without losing coverage? | Every new rule makes the previous ones less likely to be followed. |
| **[agent-command-guard-hardening](https://github.com/cengcengcai-creator/agent-command-guard-hardening)** | Does my command interceptor actually block, and what does it miss? | You assume the guard works until something gets through. |
| **[agent-behavior-baseline](https://github.com/cengcengcai-creator/agent-behavior-baseline)** | What is normal for this agent, and did it change? | Every behavior change is noticed late, by accident. |
| **[deep-research-discipline](https://github.com/cengcengcai-creator/deep-research-discipline)** | Can someone else reproduce and check this conclusion? | Confident output with no way to audit it. |

---

## How they fit together

They are a pipeline, not a bundle:

```
write rules  →  slim them  →  enforce them  →  observe them  →  review conclusions
     │              │               │                │                  │
  (authoring)   slimming      guard-hardening    baseline        research-review
```

- **Write → slim.** Adding a rule makes the file longer; slimming keeps the per-rule attention budget constant. Joint accounting (slim + moved-out files >= original) is the acceptance test.
- **Slim → enforce.** A short rule can be gated. A long paragraph cannot. Enforcement audit tells you which rules are mechanically checkable; the rest are prompts.
- **Enforce → observe.** A gate answers *may this happen?* A baseline answers *did behavior change?* You need both — neither is sufficient.
- **Observe → review.** Once you measure, someone has to check the measurement. Producer and reviewer must be different agents.

---

## Design principles

1. **Structure beats prompting.** A rule enforced by a missing tool cannot be forgotten; a rule written in a prompt can. When a rule keeps failing, move the constraint out of the prompt and into the system.
2. **Falsifiable or it did not happen.** Every check needs a negative control: prove the checker fires on a known-bad sample before trusting a clean result.
3. **Measure before optimizing.** "Feels better" is not a result. Fix the metric definition first — most "improvements" are denominator changes.
4. **Subtraction is the hardest lever.** Adding a rule is cheap and looks productive. Deleting one requires knowing it never fired. Build the delete-list first.

---

## Install

```bash
clawhub install agent-rule-enforcement
clawhub install agent-instruction-slimming
clawhub install agent-command-guard-hardening
clawhub install agent-behavior-baseline
clawhub install deep-research-discipline
```

Each skill is self-contained — they are useful individually. Start with the one matching your current pain, not with all five.

---

## When *not* to use these

- **You have under ~30 rules.** Governance overhead exceeds the benefit. Add rules first, come back when attention dilution starts.
- **Your agent is single-purpose and short-lived.** No accumulation, so no drift.
- **You want a library of prompts.** These are process skills with scripts and acceptance criteria, not prompt collections.

---

## Provenance

Extracted from a personal OpenClaw deployment that ran continuously for months, where each skill came from a specific failure: rules that never fired, a checker that passed everything, a research report nobody could verify. The war stories are in each repository's references.

MIT-0. Use, modify, redistribute — no attribution required.

---

## 中文说明

这套技能解决的是**「规则写了但不生效」**这个问题，不是提供更多提示词。

五个技能组成一条流水线：**写规则 → 精简 → 强制 → 观测 → 复核结论**。共同主张只有一条：

> 当一条规则反复失效时，把它从提示词里搬到系统结构里——让工具缺失、让闸门拒绝，而不是继续润色措辞。

单个可用，不必五个一起装。从你当前最痛的那一段开始。
