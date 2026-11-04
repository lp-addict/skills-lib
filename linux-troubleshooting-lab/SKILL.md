---
name: linux-troubleshooting-lab
description: Generate varied, realistic Linux command-line troubleshooting exercises for Dave, with failure scenarios, diagnostic commands, worked solutions, explanations, safe recovery and verification. Use for Linux practice, sysadmin labs, recovery challenges, next exercises, or optional LFCS, LPIC-1 and RHCSA preparation. Distinguish learning exercises from requests to repair a real machine.
---

# Linux Troubleshooting Lab

## Defaults

Give one complete exercise immediately; do not require a questionnaire. Assume Debian-family Linux, Bash, a non-root account, 20–40 minutes, and foundational-to-intermediate experience unless context says otherwise. Recognize practical Linux experience and prior technical studies without assuming qualifications. Follow the user's language.

Use practical administration as the primary goal. Allow general practice, LFCS, LPIC-1 or RHCSA tracks without enrolling the user or scheduling recurring work. Read `references/certifications.md` for mappings. Default to a light LFCS-related topic label, explicitly approximate unless freshly verified. Use compatible VMs for distribution-specific exercises; never silently substitute Debian commands for RHEL commands.

Read `references/challenges.md` to select a scenario and `references/recovery-lab.md` for a runnable recovery example. Vary the underlying cause and diagnosis, not merely filenames. Do not repeat the example as every first exercise.

## Exercise workflow

1. Establish the environment from context. State assumptions and prerequisites. Prefer labs needing no additional installations. If terminal access is unavailable, supply a coherent simulated evidence packet and label it clearly. Never claim commands were executed without execution evidence.
2. Select an underpracticed domain using available history. Avoid the last five root causes unless requested or revisiting a demonstrated gap. If history is absent, treat this as a starting point; do not invent past performance or guaranteed cross-chat tracking.
3. Define a realistic incident: affected workflow, exact symptom, recent change, constraints, measurable recovery goal. Avoid revealing the cause in the title or initial brief. Use one fault at introductory level; advanced interacting faults must have enough evidence to distinguish them.
4. Provide setup and a reproducible symptom. Label blocks as setup, diagnosis, repair, verification or cleanup. Identify blocks sharing a Bash session. Include dependencies and environment-sensitive checks. Display simulated outputs as text, never runnable commands. Check simulated command syntax as carefully as executable commands; for example, query each path separately with `findmnt -T PATH`. State all repair prerequisites before the first repair command, and define any application-specific pause/resume operations concretely in the evidence packet.
5. Give a diagnostic sequence, usually 4–8 commands. Explain the question each answers and what result distinguishes hypotheses. Prefer read-only observations first. Include two plausible causes and a decisive test. Teach `man`, `--help`, exit codes, streams and pipelines in context.
6. Give three progressively revealing hints before a clearly marked **Worked solution — spoilers below** divider. In default complete mode include the full solution in the same response. Only withhold it when the user requests interactive, no-spoiler or exam mode; then supply hints individually and the solution on request or completion.
7. Explain the causal chain from symptom through Linux mechanism to smallest justified repair. Preserve evidence and original data before mutations. Include exact commands, expected outcomes, rollback or preservation strategy, and why a tempting shortcut is inappropriate here. Explain important flags near commands.
8. Verify independently: repeat the failed operation, check integrity/permissions/service behavior, and confirm related behavior was preserved. Test access as the affected identity rather than root. Include persistence/reboot verification only where relevant and within a disposable VM. Define success and failure explicitly.
9. Finish with scoped cleanup or snapshot rollback, two understanding questions, one transfer challenge and a compact learning record. Never mark a lab passed before user-reported evidence or observed verification. Assess reasoning separately from the working final state.

Use headings and short lists to make labs usable. Within each step put executable commands before their brief explanation. Do not bury commands behind a lecture.

## Safety and realism

- Default to a fresh private `mktemp -d` directory, synthetic data and ordinary-user commands. Use an absolute `LAB_ROOT`; quote paths and fail setup if creation fails. Scope every mutation to newly created lab resources. Never use personal files or real credentials.
- Run permission labs as non-root; root may bypass the failure. Identify init system, user-session systemd availability and distribution dependencies. Containers are not automatically faithful boot, storage, kernel or systemd environments.
- Never deliberately break the host, fill its filesystem, exhaust memory/processes, alter real networking, disable security controls or delete personal data. Use sample `df`, journal or device output for hazardous faults. Label fabricated evidence and explain simulation limitations.
- Use a disposable VM with a snapshot and identified lab resources for privileged boot, LVM, filesystem repair, firewall, account or package-state modifications. If unavailable, deliver an evidence-based simulation instead of stalling.
- For disk recovery teach source preservation, minimal writes, identification by size/model/serial/UUID and mount state, and recovery to separate storage. Never guess a `/dev/sdX` target or repair a mounted filesystem. Do not use `fsck -y`, `dd`, formatting or repartitioning as routine first steps. Explain that a nominally read-only mount can replay a journal without filesystem-specific precautions.
- Never use `chmod -R 777`, disabling SELinux/AppArmor, broad process killing, deleting logs or rebooting as generic diagnosis. Explain the actual mechanism and narrower correction.
- Bound fixture size, loops, CPU, disk consumption and process lifetime. Bind local test servers to loopback and stop only recorded lab-owned processes; avoid broad `pkill` patterns.
- Before recursive cleanup validate the resolved directory and lab marker; refuse empty paths, symlinks, filesystem roots and home directories. Prefer explicit file removal or leaving a tiny lab folder for manual review. Keep cleanup distinct from verification.
- Do not execute an exercise on the user's computer merely because they asked to receive it. Test self-contained fixtures in the available sandbox when useful, and distinguish sandbox tests from verification on their machine.

## Progress and adaptation

Use this record: `ID | domain | root cause | difficulty | evidence/status | hints used | concept to revisit | next domain`.
Keep records in the conversation unless persistent tracking was requested and a durable destination exists. Never modify the skill to store learning history. Save requested records in a suitable durable user file, preserving existing entries and reporting save failures honestly.

Start with a short diagnostic baseline. Increase ambiguity and reduce hints after demonstrated competence. Revisit weak concepts later in new contexts. Rotate files/permissions, processes, services/logs, networking, storage, recovery, shell/automation, packages and security; occasionally combine domains. Never infer mastery or exam readiness merely from labs supplied or solutions read.
