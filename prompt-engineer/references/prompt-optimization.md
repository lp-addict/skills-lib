# Repair a prompt from observed behavior

Capture the request, current prompt, one failing input and output, and the desired behavior when available. Preserve the original wording and interface that already work. Separate hard constraints from defaults and taste. If an example is hypothetical, identify it as such.

| Symptom | First check | Targeted change |
|---|---|---|
| Includes plausible but irrelevant results | Inclusion rule is subjective or lacks a boundary | Define a positive and a near-miss case. |
| Omits an important exception | General rule and exception overlap | State their scope and precedence explicitly. |
| Output parses but contains false facts | Schema restricts shape but leaves evidence unspecified | Name the evidence required for each field and define unknown behavior. |
| Style sounds generic | Persona adjectives do not describe observable writing | Supply a short authentic example or specify voice, audience and what to preserve. |
| Task stalls for missing information | Small uncertainties are treated like blockers | Define a reasonable default or one question for a consequential ambiguity. |
| Agent reports successful actions without evidence | Prompt assumes access or leaves completion undefined | Name the available tool, required verification and fallback when unavailable. |

**Example.** A bug triage prompt says “flag urgent crashes” and marks a closed ticket as urgent because its title mentions a crash. If the queue is for unresolved incidents, define an eligible ticket by current status before assigning a severity level. Check a resolved crash and an active incident whose title never says “crash.”

Use one targeted edit while diagnosing an isolated failure. When defects share a cause or the old design cannot express the user's request, rewrite the relevant section together and recheck each preserved behavior. Remove generic advice, redundant examples and unsupported demands. Keep extra words that establish units, precedence, evidence or a consequential fallback.

Compare original and candidate on the same inputs, then use a fresh example before adopting a production change. Track quality first; measure tokens, latency and cost only when relevant to selecting among viable candidates. See [evaluation-frameworks.md](evaluation-frameworks.md) for comparison methods.
