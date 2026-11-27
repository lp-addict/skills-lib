# Reviewing Vinted Vinyl

Start with [SKILL.md](SKILL.md) for operation selection. The [batch guide](references/batch-processing.md) owns queue order and delivery. The [archive workflow](references/archive-workflows.md) and [storage specification](references/archive-specification.md) cover save recovery, file identity and listing-register updates. The [listing format](references/listing-format.md) defines the two copyable fields.

## Synthetic walkthroughs

Use invented records A, B and C and a disposable archive or a read-only simulation. Capture the queue order, output sections, intended writes and verified writes separately.

| Input | Expected observation |
| --- | --- |
| A front, B label, A back, then a duplicate A back | Two jobs in A/B order. Both distinct A photos stay with A. The duplicate creates no stock. |
| C arrives while A is processing and B is queued | Finish A, then B, then C. Deliver the three sections together. |
| A grade correction arrives after A was saved but before delivery | Reopen the original job, reuse the entry and emit A once with the corrected grade. |
| A cannot be identified and B can | Keep A's question at position one. Deliver B at position two with its own archive outcome. |
| A save times out after the provider accepts it | Read the current manifest and register, reuse matching IDs and rows, and report any unresolved result. |
| Request a preview, then request posting text for a positively identified copy | Preview causes zero writes. For posting text, verify the configured register, preserve unrelated fields and read back the exact text. A repeated identical request creates no extra row. |
| Ask for posting text with no verified register destination | Return usable text and report the missing destination. Do not guess a sheet or claim the row was saved. |

These are acceptance scenarios for a reviewer. Live Drive write recovery and host delivery of follow-up messages require an integration run. A prepared listing does not establish a publication or sale.
