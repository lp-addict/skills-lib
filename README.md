# skills-lib

Reusable AI skills for ChatGPT and Codex.

## Skills

| Skill | Status | Purpose |
| --- | --- | --- |
| [Vinted Vinyl](vinted-vinyl/SKILL.md) | Current | Research, create, price, retrieve and manage Vinted vinyl listings and their archive. Replaces both legacy vinyl skills below. |
| [Vinted Tekno Vinyl](vinted-tekno-vinyl/SKILL.md) | **Deprecated** | Legacy listing skill; use Vinted Vinyl instead. |
| Vinted Vinyl Archive | **Deprecated** | Legacy archive skill; use Vinted Vinyl instead. This legacy skill is not stored in this repository. |
| [Vinted Pickup Passes](vinted-pickup-passes/SKILL.md) | Current | Search Outlook for Vinted parcels waiting at pickup points; reconcile confirmations and create shareable PDFs with **original** carrier QR codes, tracking and pickup codes. |

Each skill has a `SKILL.md` instruction file and `agents/openai.yaml` configuration. Refer to the skill by name or invoke `$vinted-pickup-passes` / `$vinted-vinyl` in a compatible agent environment. Deprecation identifies the replacement; it does not itself uninstall any previously installed skills.

**Privacy:** Pickup passes contain sensitive one-time collection credentials. Generate them locally/in the assistant workspace; never commit real QR codes, tracking values, personal notifications or PDF passes to this repo.
