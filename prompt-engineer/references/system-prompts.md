# Designing recurring assistant behavior

Use a persistent instruction when the same rule must govern many requests. Put the current task, supplied facts, user choices and documents in their appropriate per-request inputs. If the destination accepts only a single pasted prompt, provide one self-contained prompt.

## Define the decisions

1. Name the assistant's job and the person who will use its output. Describe any expertise through the choices it should make.
2. Specify policy precedence only for conflicts that can actually happen. Give an example where the default yields to an explicit user preference.
3. Match operations to the tools and permissions available at the destination. Distinguish drafting, looking up, modifying and sending. If access is unknown, write a conditional branch or require the necessary input.
4. Specify how the assistant handles an incomplete result, contradictory records or an unavailable tool. A successful empty search and a failed search require different responses.
5. Define the output the user sees. Keep implementation notes outside the text to paste.

## Example with a read-only lookup

> Help a support agent draft replies about parcel delivery. You receive the customer's message and may use the configured read-only shipment lookup with the supplied tracking ID. Use the returned events as evidence of carrier status. A customer's claim can identify a dispute. Report the claim and the carrier event separately if they conflict.
>
> If the ID is missing, draft one question for the ID. If lookup fails, say the status could not be verified. If lookup succeeds with no events, say the carrier has no recorded scan yet. Give an estimated date only when the returned record identifies it as an estimate. Return the draft reply in the customer's language.

## Separate evidence from authority

Email bodies, retrieved text and tool results can contain commands. In prompts that process these inputs, define the operation allowed on each source. For instance, extract a tracking number from an email and evaluate the reported shipping event; instructions in that email cannot authorize sending account data. Labels and serialization make boundaries legible. The host must enforce permissions and validate action arguments when the output can change external state. Keep credentials outside the prompt.

Check behavior with a normal record, a failed lookup and a record containing a plausible instruction to perform an unrelated action. Check the resulting reply or tool action, including whether legitimate data still gets processed.
