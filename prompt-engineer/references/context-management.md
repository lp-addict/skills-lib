# Selecting context for a prompt

<!-- Earlier context guidance adapted from PR #168 by Genius-apple (https://github.com/Jeffallan/claude-skills/pull/168). -->

Work backwards from the answer. Which facts could change the decision? Include those facts with enough surrounding text to preserve qualifications, scope, units and exceptions. Preserve exact identifiers, quotes or code when the recipient must match them.

## Assemble the input

- Keep the current question and binding constraints easy to locate. Separate retrieved records, examples and previous decisions using source labels or supported message roles.
- Retain source IDs, dates, relevant scope and whether a record is complete. A trusted instruction can define precedence by authority or effective date. The newest timestamp alone need not establish which policy applies.
- Remove duplicates and irrelevant history first. When compressing, preserve corrections, negation, unresolved conflicts, dependencies and open questions. Retain a pointer to the full source when the target can retrieve it.
- When the available text is partial, specify what conclusion can be drawn from that portion. To answer “all orders,” establish coverage or label the result partial.
- If the task allows general knowledge as well as source evidence, tell the model where inference is acceptable and how to label it. For a closed-source task, leave missing facts unknown.

## Example of preserving scope

A policy says returns are allowed for 30 days. An amendment says 14 days for clearance items purchased from 1 June. A summary such as “the new return period is 14 days” loses the category and effective date. Supply the two clauses with source IDs and ask:

> Given the purchase date and sale category, identify the applicable return period from the supplied clauses. Cite the clause ID. If a missing date or category could change the result, state exactly which fact is needed. If both possible periods lead to the same answer to the user's question, give that answer with the shared condition.

## Diagnose context failures

First check that the necessary source was retrieved intact. Run the same question with the relevant passage alone and with the full input. A difference suggests investigating conflicting evidence, formatting or distractions. Move a decisive exception to another position and retest; measure what the target actually does. Check for invented sources, correct resolution of updates, and false certainty when a source is removed.

For repeated agent turns, retain a short record of the active goal, user decisions, verified actions with source pointers, remaining work, and failures. Rebuild it when those facts change. If trimming tool output, select relevant records across the entire result and mark omissions rather than silently keeping the first few lines.
