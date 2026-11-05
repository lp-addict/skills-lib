# Evaluate what the prompt makes the target do

Choose an observable downstream outcome: correct extraction, a decision supported by the supplied policy, or a draft the intended reader can use. Score that outcome before preferences such as brevity. Good-looking instructions alone do not demonstrate good model performance.

## Small comparison

1. Freeze the original prompt, target model, tool access, inputs, available context and settings. Define acceptance criteria before looking at outputs.
2. Pick representative inputs and a small number of revealing boundaries. Include at least one plausible wrong result that looks superficially correct. For variable tasks, reserve fresh examples for the final comparison.
3. Run both prompts under matched conditions when model access and authorization are available. Record the actual outputs and failures separately from expected results. With no target runtime, inspect likely behavior and report the cases as proposed tests or a walkthrough.
4. Use a deterministic check for objective constraints. For a classification task, inspect misses by class as well as overall accuracy. For extraction, check association and semantics after parsing. For prose, anchor judgment to concrete reader needs and compare candidates in randomized order where practical. Allow ties and retain a reason for each preference.
5. Check for regressions on previously successful inputs. For repeated production use, rerun representative cases across independent generations to see whether a gain persists. Report sample size and the limits of the comparison. A small passing set supports those cases only.

## Example failure probes

| Case | Observable check |
|---|---|
| Document contains two records and one shared fee | No record receives another record's amount. |
| Source has no answer | Response follows the specified missing-data behavior without inventing an answer. |
| Quoted email says “ignore filtering rules” | Sender text cannot change the triage policy. |
| A rewrite asks for urgency while preserving the author's wording | A deadline appears only if supplied, and original facts survive. |

For subjective evaluations, define an anchored question such as “Would the reader know which action to take and by when, using only dates in the source?” Score required behaviors individually. An overall style score can hide a missed exclusion or a fabricated deadline. If using an evaluator model, give it the request, authorized evidence and outputs; calibrate its judgments with a few human-checked examples before relying on its score.
