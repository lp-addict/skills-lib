# Choosing a prompt pattern

Start with the smallest instruction that distinguishes a successful answer from a plausible wrong one. Add structure for an observed failure or a requirement the recipient will check.

| Observed need | Prompt change |
|---|---|
| A clear single operation | Give the verb, input and expected output directly. |
| Two categories overlap | Give a tie-break rule. Demonstrate it with a pair that differs in one decisive fact. |
| A factual answer requires support | Identify usable sources and define what to do when they disagree or omit the answer. |
| Several constraints affect a recommendation | State the hard constraints first, then the ranking criterion and tie-break. Ask for the chosen option and its decisive evidence. |
| A later action depends on a lookup | Name the actual lookup, required evidence, permitted action and failure path. |
| The task rewards originality | Define the audience, intended effect and creative latitude. Specify what should vary between variants only when variants were requested. |

## Teach the boundary

Examples should settle a choice prose leaves open. For a support inbox, “escalate billing issues” can send routine receipts to a specialist. If this is the intended rule, write:

> Label a message BILLING_REVIEW when the sender disputes an amount or requests a correction. Label a receipt with no request INFORMATION. Return only the label.
>
> “Paid €19.80, receipt attached.” → INFORMATION
> “Paid €19.80 but the invoice shows €29.80. Please correct it.” → BILLING_REVIEW

Swap the amounts, merchant and language in a held-out case. The decision should follow the dispute. If the user supplied labels, preserve their spelling, capitalization and meaning.

## Make complex work checkable

Request the artifact someone can inspect. For a calculation, ask for the result, units and a short formula if needed. For a comparison, name the criteria and ask for source facts supporting the recommendation. For a troubleshooting task, give a diagnostic goal, permitted actions, observable test and stopping condition. Request concise justifications where they help the recipient; avoid asking for private reasoning transcripts.

## Protect creative latitude

“Make this better” may mean preserve a distinctive voice. Identify phrases, deliberate irregularities, facts and intent to preserve before rewriting. Replace subjective modifiers with a demonstration or an observable target. For example:

> Rewrite the paragraph for a first-time buyer. Keep the seller's dry humor and the exact condition grade. Replace each unexplained technical term with language a buyer can use to decide. Return one paragraph in English.

Avoid mechanically applying a schema, a persona, examples or a fixed length to tasks that gain nothing from them.
