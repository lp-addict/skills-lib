# skills-lib

Reusable AI skills for ChatGPT and Codex.

## Skills

| Skill | Purpose |
| --- | --- |
| [Vinted Tekno Vinyl](vinted-tekno-vinyl/SKILL.md) | Research and create Dutch Vinted listings with condition and price guidance. |
| [Vinted Pickup Passes](vinted-pickup-passes/SKILL.md) | Search Outlook for Vinted parcels waiting at pickup points; reconcile confirmations and create shareable PDFs with **original** carrier QR codes, tracking and pickup codes. |

Each skill has a `SKILL.md` instruction file and `agents/openai.yaml` configuration. Refer to the skill by name or invoke `$vinted-pickup-passes` / `$vinted-tekno-vinyl` in a compatible agent environment.

**Privacy:** Pickup passes contain sensitive one-time collection credentials. Generate them locally/in the assistant workspace; never commit real QR codes, tracking values, personal notifications or PDF passes to this repo.
