# Linux Enumeration Cheat Sheet

## Overview

A checklist of standard manual enumeration commands used after gaining a foothold on a Linux target, to identify the current context, users, permissions, and potential privilege-escalation vectors.

> [!NOTE]
> This is a general-purpose reference sheet. The specific enumeration actually performed in this repository's lab notes is documented in [`docs/06-Initial-Access`](../docs/06-Initial-Access/README.md) and [`docs/07-Privilege-Escalation`](../docs/07-Privilege-Escalation/README.md).

## Common Syntax

Most of these are standalone, single-purpose commands rather than a single unified tool invocation.

## Useful Commands

| Command | Description |
|---|---|
| `whoami` | Current effective user |
| `id` | Current user's UID/GID and group memberships |
| `hostname` | Target hostname |
| `uname -a` | Kernel version and architecture |
| `cat /etc/os-release` | Distribution name and version |
| `ls -la` | Directory listing including hidden files and permissions |
| `sudo -l` | Commands the current user may run via `sudo` |
| `find / -perm -4000 -type f 2>/dev/null` | Locate SUID binaries |
| `find / -perm -2000 -type f 2>/dev/null` | Locate SGID binaries |
| `getcap -r / 2>/dev/null` | Locate files with Linux capabilities set |
| `crontab -l` | Current user's scheduled cron jobs |
| `cat /etc/crontab` | System-wide cron jobs |
| `ps aux` | Running processes |
| `netstat -tulpn` / `ss -tulpn` | Listening network services |
| `cat /etc/passwd` | List local user accounts |
| `history` / `cat ~/.bash_history` | Command history (may reveal credentials or hints) |

## Important Options

| Option/Flag | Description |
|---|---|
| `-la` (with `ls`) | Long format, including hidden (dotfile) entries |
| `2>/dev/null` (with `find`) | Suppress permission-denied noise |
| `-perm -4000` (with `find`) | Match files with the SUID bit set |

## Examples

```bash
# Quick context check after gaining a foothold
whoami; id; hostname; uname -a

# Check sudo privileges
sudo -l

# Hunt for SUID/SGID binaries
find / -perm -4000 -type f 2>/dev/null
find / -perm -2000 -type f 2>/dev/null

# Check for writable files owned by root, referenced in sudoers
ls -la <path-to-sudo-permitted-script>
```

## Common Mistakes

- Only checking `sudo -l` output for the *command name* and missing that the permitted path might be user-writable.
- Skipping hidden files (`.bash_history`, `.ssh/`, dotfiles in general) during home-directory review.
- Not cross-referencing kernel/OS version against known local privilege-escalation exploits before assuming a misconfiguration-based path is the only option.

## References

- [GTFOBins](https://gtfobins.github.io/) — reference for abusing Unix binaries for privilege escalation once a relevant binary/permission is identified

## Key Takeaways

- Enumeration should be systematic: identity → permissions → filesystem → processes → scheduled tasks → history.
- The most common real-world privilege-escalation vectors (misconfigured `sudo` rules, writable root-owned scripts, SUID binaries) are all discoverable through a short, repeatable command checklist.
