# Skills Library

Reusable AI skills for ChatGPT and Codex, covering vinyl listings, Vinted parcel collection, Jungian reflection and Linux troubleshooting practice.

## Available skills

| Skill | What it does | Status |
| --- | --- | --- |
| [Linux Troubleshooting Lab](linux-troubleshooting-lab/SKILL.md) | Creates varied, safe Linux failure-diagnosis and recovery exercises with diagnostic commands, hints, worked solutions, verification and optional LFCS, LPIC-1 or RHCSA alignment. | Current |
| [Jungian Reflection](jungian-reflection/SKILL.md) | Explains Jungian ideas and supports reflective exploration of dreams, symbols, shadow work and Jungian texts in English or Dutch. | Current |
| [Vinted Vinyl](vinted-vinyl/SKILL.md) | Researches and prices records, prepares Vinted listings, and finds, saves or updates listings and photos in the existing archive. | Current |
| [Vinted Pickup Passes](vinted-pickup-passes/SKILL.md) | Finds Vinted parcels awaiting collection in Outlook and prepares a PDF with verified original carrier QR codes and pickup details when available. | Current |
| [Vinted Tekno Vinyl](vinted-tekno-vinyl/SKILL.md) | Earlier vinyl listing skill, retained for reference. Use Vinted Vinyl for new work. | **Deprecated** |

The former **Vinted Vinyl Archive** skill is also deprecated and replaced by Vinted Vinyl. Its separate skill files are not stored in this repository. Deprecation does not automatically uninstall previously installed skills.

## Using a skill

Open a skill's `SKILL.md` for its instructions and requirements. In a compatible environment where it is installed, invoke it by name, for example `$linux-troubleshooting-lab`, `$jungian-reflection`, `$vinted-vinyl` or `$vinted-pickup-passes`. Availability of connected services and tools depends on the environment.

Vinted Vinyl contains focused reference files for research, listing format, pricing and archive operations. It uses the configured existing archive; the repository is not the archive itself.

Jungian Reflection includes seven reference guides for concepts, dreams and symbols, shadow work, active imagination, recovery, visual mapping and reading sources. It offers education and reflection with tentative interpretations. Its source attribution and MIT license notice are included in the skill directory.

Linux Troubleshooting Lab defaults to Debian/Bash and 20–40-minute exercises. It includes 15 incident types, a tested recovery example and certification guidance. Request a complete worked exercise or interactive hints; hazardous host-level failures use simulated evidence or disposable VMs. Progress uses available conversation history unless persistent tracking is requested.

## Repository layout

- `linux-troubleshooting-lab/` — Linux troubleshooting skill, challenge catalogue, recovery example, certification guidance, agent configuration and icon.
- `jungian-reflection/` — Jungian reflection skill, seven reference guides, agent configuration, icon and license.
- `vinted-vinyl/` — unified vinyl skill, references and configuration.
- `vinted-pickup-passes/` — parcel lookup and pickup-pass skill.
- `vinted-tekno-vinyl/` — deprecated predecessor retained for reference.
- [CONTRIBUTING.md](CONTRIBUTING.md) — conventions for adding, updating and documenting skills.

Each skill has a `SKILL.md` and agent configuration; supporting references and assets are included where needed.

## Privacy

Pickup passes may contain one-time collection credentials. Never commit real QR codes, tracking values, personal notifications, email content or generated pickup PDFs. Keep credentials and secrets out of the repository; review configuration and examples before sharing them.
