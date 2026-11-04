# Contributing

Keep skills useful, accurate and easy to find. This repository's main page is [README.md](README.md); there is no separate `MAIN.md`.

## When adding or changing a skill

1. Place each skill in its own clearly named directory with a `SKILL.md` describing its purpose, triggers and workflow. Include agent configuration and only the references or assets the skill needs.
2. Keep instructions consistent with the actual skill behavior. Link to supporting files using valid relative paths and avoid duplicating rules that have one authoritative home.
3. Check examples and configuration for secrets or personal data. Never commit live pickup codes, QR codes, tracking numbers, emails, or generated pickup PDFs.
4. Validate the skill and test the affected workflows when practical. Describe what was checked and any remaining limitations.
5. **Update [README.md](README.md) in the same change** whenever a skill is added, renamed, deprecated, removed or materially changes purpose, behavior, status or structure. Update the skills table, links, usage and layout as needed. Keep the README accurate even if a skill is not stored in this repository.
6. For a deprecated skill, name its replacement and preserve historical files unless their removal is explicitly requested. Do not claim that a documentation change uninstalls an existing skill.

## Before finishing

- Confirm every listed skill, status and relative link matches the repository.
- Check that `SKILL.md`, agent configuration and referenced files are present and consistent.
- Review the change for sensitive information, unrelated edits and broken links.
- Record relevant validation and known limitations in the change description.
