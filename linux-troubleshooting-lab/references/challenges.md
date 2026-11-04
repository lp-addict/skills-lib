# Challenge selection

Generate original incidents from this menu. Commands are diagnostic candidates, not universal fixes. For simulations provide complete, consistent evidence and clearly hypothetical repairs confined to a fictional VM.

| ID | Incident and competing causes | Useful evidence | Safe environment and success |
|---|---|---|---|
| FILE-01 | Export unavailable: damaged contents versus stale symlink | `ls -l`, `readlink`, `stat`, `file`, `cmp`, `sha256sum` | Private tiny fixtures; restore bytes and preserve evidence |
| PERM-01 | Config unreadable: file mode versus directory traversal | `id`, `namei -l`, `stat`, optional `getfacl` | Non-root fixture; minimal permissions restore original identity's access |
| SHELL-01 | Scheduled script fails: PATH versus working directory | `command -v`, `pwd`, restricted `env`, exit status | Subshell fixture; explicit paths without global profile changes |
| PROC-01 | Missing output: redirection versus producer failure | `ps`, `wc`, `PIPESTATUS`, subshell `pipefail` | Finite producer; detect failure and preserve stderr |
| SERVICE-01 | Service exits: bad ExecStart versus config error | `systemctl status`, `systemctl cat`, `journalctl -u` | Evidence packet or systemd VM; stable functional service |
| NET-01 | Name fails, IP works: DNS versus hosts override | `getent hosts`, `ip route`, optional `resolvectl` | Evidence packet or VM; resolution and request both work |
| NET-02 | Local API unreachable: bind address versus port | `ss -ltn`, loopback `curl`, process command line | Bounded local process or evidence; correct endpoint works |
| SPACE-01 | Writes fail despite free bytes: inodes versus quota | `df -h`, `df -i`, `du`, quota evidence | Simulated output only; identify exhausted resource |
| SPACE-02 | Deleted log occupies space: open descriptor versus wrong mount | `df`, scoped `du`, optional `lsof +L1` | Simulated output; controlled reopen plan preserving evidence |
| BOOT-01 | Emergency boot: stale fstab UUID versus filesystem damage | `lsblk -f`, `blkid`, `findmnt`, journal, fstab fixture | Evidence or snapshot VM; correct mapping and VM boot |
| RECOVERY-01 | Interrupted update damaged an inventory export | `stat`, `sha256sum`, `tar -tf`, `cmp` | recovery-lab.md; stage restore, preserve original, verify integrity |
| PKG-01 | Package operation blocked: active process versus interrupted state | Process list, package logs, `dpkg --audit` | Evidence or VM; never default to deleting lock files |
| SECURITY-01 | Modes look right: ACL mask versus MAC policy | `getfacl`, identity, audit logs, optional `ls -Z` | Evidence or compatible VM; intended access without disabling enforcement |
| STORAGE-01 | Restored filesystem empty: wrong mount versus overwritten data | `findmnt`, `lsblk`, namespace and device identity | Evidence or VM disks; locate data before writes |
| TIME-01 | TLS or timing fails: clock skew versus certificate/timezone mismatch | `date -Is`, `timedatectl`, certificate dates | Evidence packet; do not change host clock |

Foundation: one fault, full diagnostic toolbox, 15–25 minutes. Intermediate: competing hypotheses and partial toolbox, 25–40 minutes. Advanced: interacting faults, preservation constraints, 40–60 minutes; keep hazardous changes simulated unless a VM exists.

Evaluate evidence collection, hypothesis testing, least-invasive repair, explanation and independent verification separately. Guessing a working fix does not establish competence. Make time limits optional.
