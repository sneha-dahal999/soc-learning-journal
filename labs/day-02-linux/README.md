# Day 2 Lab — Linux File Management and Integrity

**Completion date:** 4 September 2026
**Environment:** Ubuntu 24.04 LTS through WSL2

## Objective

Practise Linux navigation, file management, permissions, text searching, safe deletion and SHA-256 integrity verification.

## Tasks Completed

- Navigated through Linux directories using `cd` and `pwd`
- Created directories with `mkdir -p`
- Created files using `touch`
- Added and displayed text using `echo` and `cat`
- Copied and renamed files using `cp` and `mv`
- Searched file contents using `grep`
- Examined file details and permissions using `ls -l`
- Restricted evidence files using `chmod 600`
- Generated and verified SHA-256 checksums
- Detected an intentional file modification
- Restored the original file and confirmed its integrity
- Safely deleted a disposable file using `rm -i`

## Integrity-Test Result

The original checksum verification returned `OK`. After text was intentionally appended, verification returned `FAILED`. Restoring the trusted copy caused verification to return `OK` again.

This demonstrated that a SHA-256 hash can detect a change in file content.

## Permission Learned

`-rw-------` means only the file owner can read and write the file. The group and other users have no access.

## Security Lessons

- Confirm the current directory before changing or deleting files.
- A hash checks integrity but does not control access.
- File permissions control access but do not prove integrity.
- Evidence and its checksum record should both be protected.
- Never test or modify systems without authorisation.
- Use cautious deletion commands and verify the exact target.

## Commands Practised

`pwd`, `ls`, `cd`, `mkdir`, `touch`, `echo`, `cat`, `cp`, `mv`, `grep`, `chmod`, `sha256sum`, `rm`
