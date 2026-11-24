# FTP Cheat Sheet

## Overview

FTP (File Transfer Protocol) is a plaintext-by-default protocol for file upload/download, typically running on TCP/21 (control) with a separate data connection (active or passive mode).

> [!NOTE]
> This is a general-purpose reference sheet. No FTP service was present on the target documented in this repository — see [`docs/04-FTP`](../docs/04-FTP/README.md) for details.

## Common Syntax

```bash
ftp <target>
```

## Useful Commands

| Command | Description |
|---|---|
| `nmap -p21 -sV <target>` | Identify the FTP daemon and version |
| `ftp <target>` | Open an interactive FTP session |
| `ftp> anonymous` / password: any/blank | Attempt anonymous authentication |
| `ftp> ls` / `ftp> dir` | List remote directory contents |
| `ftp> get <file>` | Download a file |
| `ftp> put <file>` | Upload a file (if write access is permitted) |
| `ftp> binary` | Switch to binary transfer mode (recommended for non-text files) |
| `ftp> passive` | Toggle passive mode |

## Important Options

| Script | Description |
|---|---|
| `ftp-anon` | Checks whether anonymous FTP login is permitted |
| `ftp-bounce` | Tests for the FTP bounce attack vector |
| `ftp-syst` | Attempts to retrieve system information via the `SYST` command |

## Examples

```bash
# Identify the FTP service
nmap -p21 -sV -sC target

# Manual anonymous login check
ftp target
# Username: anonymous
# Password: anonymous
```

## Common Mistakes

- Forgetting to switch to `binary` mode before transferring non-text files, corrupting the transfer.
- Not testing both active and passive modes when transfers hang behind NAT/firewalls.
- Assuming anonymous access is disabled without explicitly testing it.

## References

- [RFC 959 — File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)

## Key Takeaways

- Always test anonymous authentication explicitly; it remains a common misconfiguration.
- Cross-reference the FTP daemon/version banner against known CVEs only after confirming the version through direct interaction.
