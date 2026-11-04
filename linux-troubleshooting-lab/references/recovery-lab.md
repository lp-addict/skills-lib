# RECOVERY-01: The inventory import stopped working

Use this as a complete example; adapt the presentation to the requested mode. All commands use Bash in the same terminal, with GNU coreutils and tar on Linux. No sudo, network, real disks or personal files are needed. The fixture uses less than 100 KB. A disposable ordinary-user session is preferable.

## Incident

A nightly inventory import rejected its CSV following an interrupted export. Recover the last known-good version while retaining the failed file for investigation. Determine whether the live file or the saved backup is unusable. Success means the restored file matches the previously recorded checksum and the failed version remains available.

This is a synthetic interrupted-write incident. It models restoring a trusted local snapshot, not undeleting blocks or repairing hardware. Setup necessarily contains the injected fault; skip reading its details if attempting the diagnosis blind.

## Setup

```bash
LAB_ROOT=$(mktemp -d /tmp/linux-lab.XXXXXXXX) || exit 1
export LAB_ROOT
mkdir -- "$LAB_ROOT/live" "$LAB_ROOT/backup" "$LAB_ROOT/staging" "$LAB_ROOT/evidence"
printf '%s\n' 'linux-troubleshooting-lab:RECOVERY-01' > "$LAB_ROOT/lab-marker"
printf 'release,quantity\nTEK-001,2\nTEK-002,1\n' > "$LAB_ROOT/live/inventory.csv"
(cd "$LAB_ROOT/live" && sha256sum inventory.csv) > "$LAB_ROOT/backup/inventory.sha256"
tar -cf "$LAB_ROOT/backup/inventory.tar" -C "$LAB_ROOT/live" inventory.csv
printf 'release,quantity\nTEK-001,' > "$LAB_ROOT/live/inventory.csv"
printf 'Lab folder: %s\n' "$LAB_ROOT"
```

The setup writes only new synthetic files beneath the printed private folder. The following read-only check reproduces the integrity failure; its nonzero exit status is expected.

## Diagnosis

```bash
ls -l -- "$LAB_ROOT/live/inventory.csv" "$LAB_ROOT/backup/inventory.tar"
stat -- "$LAB_ROOT/live/inventory.csv"
cat -- "$LAB_ROOT/live/inventory.csv"
(cd "$LAB_ROOT/live" && sha256sum -c "$LAB_ROOT/backup/inventory.sha256")
printf 'Integrity-check exit status: %s\n' "$?"
tar -tf "$LAB_ROOT/backup/inventory.tar"
tar -xOf "$LAB_ROOT/backup/inventory.tar" inventory.csv
```

`ls` tests existence and file sizes; `stat` inspects metadata without changing content. `cat` shows the incomplete record. `sha256sum -c` compares current bytes to the previously recorded digest: expect `inventory.csv: FAILED` and status 1. `tar -tf` lists the archive; expect only `inventory.csv`. `tar -xOf` sends that member to stdout rather than overwriting the live file. It shows two complete records, but visual inspection alone is not integrity verification. Consult `man tar` for the distinction between `-t`, `-x` and `-O`.

Hints:
1. Separate a missing/unreadable file from a readable file containing wrong bytes.
2. Compare current bytes to the baseline taken before the interrupted export.
3. Verify a staged restore before replacing the live file.

## Worked solution — spoilers below

The live file was truncated during rewriting. The archive contains the complete version. A file can exist, remain readable and have normal permissions while its contents are invalid. Changing permissions cannot restore bytes. The trusted pre-incident checksum provides an independent comparison for the staged data.

## Repair

```bash
cp -p -- "$LAB_ROOT/live/inventory.csv" "$LAB_ROOT/evidence/inventory.failed.csv"
if tar -xf "$LAB_ROOT/backup/inventory.tar" -C "$LAB_ROOT/staging" inventory.csv &&
   (cd "$LAB_ROOT/staging" && sha256sum -c "$LAB_ROOT/backup/inventory.sha256"); then
    cp -p -- "$LAB_ROOT/staging/inventory.csv" "$LAB_ROOT/live/inventory.csv"
else
    printf '%s\n' 'Restore verification failed; live file was not replaced.' >&2
fi
```

Run the repair block once in this new lab. `cp -p` preserves the original mode and timestamps in the evidence copy. The explicit archive member and private staging directory avoid restoring unrelated paths. The replacement happens only after extraction and digest verification both succeed. This archive was created locally by the fixture; do not apply the extraction recipe blindly to untrusted archives. For a real application, coordinate writers and consider an application-appropriate atomic replacement; this small copy is not a general concurrent-write recovery strategy.

The original failed version remains in `evidence/inventory.failed.csv`; preserve it for investigation. The original backup remains untouched. Restoring that failed copy to the live path would roll back the repair, but is unnecessary for this exercise.

## Verification

```bash
(cd "$LAB_ROOT/live" && sha256sum -c "$LAB_ROOT/backup/inventory.sha256")
printf 'Restored-file check exit status: %s\n' "$?"
cat -- "$LAB_ROOT/live/inventory.csv"
wc -l -- "$LAB_ROOT/live/inventory.csv"
if cmp -s -- "$LAB_ROOT/live/inventory.csv" "$LAB_ROOT/evidence/inventory.failed.csv"; then
    printf '%s\n' 'FAIL: the live file is still identical to the failed copy.'
else
    printf '%s\n' 'Inspect the preserved evidence below; it should contain the partial export.'
fi
cat -- "$LAB_ROOT/evidence/inventory.failed.csv"
(cd "$LAB_ROOT/staging" && sha256sum -c "$LAB_ROOT/backup/inventory.sha256")
```

Expect live checksum `OK`, status 0, the header and two complete rows (three newline-terminated lines), the preserved partial export, and staged checksum `OK`. The modeled importer criterion is byte integrity against the original export; no real importer was executed. Check errors explicitly: a failed `cmp` can mean a missing file as well as different content, so the evidence read is part of verification.

## Cleanup and reflection

Leave the tiny folder in place for inspection; it has no running processes or host configuration to undo. Print its location with `printf '%s\n' "$LAB_ROOT"`. Remove only this folder later after reviewing it; do not present unguarded recursive cleanup as a necessary step.

Ask: Why does readable not mean valid? Why must the checksum have been captured before corruption? Transfer challenge: how would you discover which files changed when an export has five files and only one is corrupt?

Record `RECOVERY-01 | file recovery | interrupted write | foundation | pending user verification | hints unknown | staged restore and integrity | permissions` until completion evidence is supplied.
