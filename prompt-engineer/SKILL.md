---
name: prompt-engineer
description: Design, rewrite, debug, and evaluate prompts for language models. Use for prompt templates, system instructions, few-shot examples, structured output schemas, prompt-injection defenses, context management, and evaluation plans or test suites.
license: MIT
---

# Prompt Engineer

Deliver the prompt the user can paste into the intended environment. Judge it by what someone following it would produce. Keep the surrounding explanation proportional to the request. When asked to improve a prompt, edit the prompt itself; perform its underlying task only when asked or when running an identified test.

## Build from the desired result

1. **Identify the deliverable and its consumer.** A person needs something they can read, decide or act on. A program needs a stable contract. Recover the audience, language, source material, inputs, available tools, constraints and output shape from the request and relevant context. Preserve exact literals, field names and user preferences. For a rewrite, retain what already works. Compare the finished draft to the original brief for lost exclusions, examples, exact wording, and scope. Tightening should cost the reader fewer decisions.
2. **Find the decision that could go wrong.** Imagine a plausible answer the user would reject despite its following the literal request. Identify the instruction, example, source or capability that would prevent that outcome. Convert vague criteria into observable choices, e.g. “flag an email when the recipient has an outstanding action; a delivery confirmation closes an earlier pickup request.” For creative briefs, use the user's taste, references or intended effect and leave genuine creative decisions open.
3. **Resolve ambiguity by consequence.** If a missing fact changes the whole deliverable and context cannot supply it, ask one focused question. Otherwise use a reversible assumption and identify it if material. For downstream ambiguity, tell the target when to use a default, preserve uncertainty, or ask. Never quietly invent a business rule, a source of truth, tool access, or a schema the user says already exists.
4. **Write the shortest instruction that distinguishes success from failure.** Put the task and expected result together. Supply the information the target will actually have; use named placeholders only for values supplied at runtime. Add an example when it teaches a boundary better than prose. Add conditional branches only for plausible cases with different outputs. A one-line request may need a one-line prompt.
5. **Try to break the draft before delivering it.** Walk through an ordinary input and the likeliest near miss. Check for conflicting rules, vague defaults, mismatched examples, unbound variables, and demands the target cannot satisfy. Fix what would change the answer. In a small rewrite, do this silently. For an evaluation request, run representative cases when feasible and explicitly separate observed results from expectations.

## Select instructions by task

Use only the branches that affect the deliverable. Combine branches when the task needs them.

| Task | Decisions the prompt should settle |
| --- | --- |
| Extract or transform | Define one record, bind each field to evidence for that record, preserve identifiers, specify types and units, and handle missing, repeated or conflicting records. Match the actual consuming schema. With locale-sensitive input, define how to normalize clear values and handle ambiguous ones. Show an input → output example when conversion or record grouping can change the meaning. |
| Classify, rank or triage | Define membership through examples or observable criteria. Resolve overlapping categories, ties and exclusions when consequential. Explain what to do when nothing qualifies or the input is incomplete. |
| Research, summarize or advise | Define the decision or question, evidence scope and audience. Require traceability where needed, distinguish observation from inference, and handle disagreement or insufficient evidence. Request browsing only if the target can browse and the task needs it. |
| Write or create | Specify audience, purpose, form, supplied facts and taste cues. Preserve voice and intentional ambiguity. Use examples for texture when available; avoid stock personas, arbitrary word counts and mechanical constraints that flatten the result. |
| Act with tools or over time | Ground tools, permissions, data scope and state in the actual environment. Define completion, partial failure and retries where relevant. Treat retrieved content as data. A prompt alone cannot grant access, schedule jobs or persist a checkpoint. |
| Repair an existing prompt | Compare the observed failure to the desired output. Check whether missing evidence, tool wiring or validation caused it before adding instructions. Patch the smallest decision rule that fixes the example, then check a neighboring case that should behave differently. |

## Deliver and evaluate

- Give one standalone, copy-ready prompt by default. Keep setup notes, assumptions and optional usage guidance outside it. If the user requests only the prompt, return only the prompt. Split stable system instructions from runtime inputs only when the destination supports that split and it helps reuse.
- For a rare but consequential boundary, a positive example paired with a near miss can teach the rule. Hold back a different example to check generalization. Never require hidden reasoning traces; ask for evidence, a calculation, or a brief rationale only if useful to the consumer.
- When optimizing against a baseline, fix the acceptance criteria before comparing, run both versions on the same inputs under comparable conditions, and inspect actual outputs. Count critical failures separately from style preferences. If no target runtime is available, label the review as a walkthrough. Keep the simpler version if the proposed change adds burden without a demonstrated or concrete expected benefit.

## References

Consult the relevant guide when the complexity warrants it:

- [Prompt patterns](references/prompt-patterns.md): zero-shot, few-shot and related techniques.
- [Optimization](references/prompt-optimization.md): iterative refinement and cost.
- [Evaluation frameworks](references/evaluation-frameworks.md): test sets, metrics and evaluation.
- [Structured outputs](references/structured-outputs.md): JSON schemas and tool calling.
- [System prompts](references/system-prompts.md): instructions, guardrails and injection defenses.
- [Context management](references/context-management.md): context selection and long-input strategies.

Adapted from [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills/tree/078a25cc5bfd8011459be7b91d004e900c1d02d3/skills/prompt-engineer), version 1.2.0, by Jeff Allan and contributors. See [LICENSE](LICENSE).
