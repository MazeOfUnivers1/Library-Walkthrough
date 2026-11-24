# 04 — FTP

## Overview

This folder exists to preserve the repository's standard methodology structure (Recon → Enumeration → Web → FTP → Credentials → Initial Access → Privilege Escalation). **No FTP service was present on the target documented in this repository** — the Nmap enumeration in [02 - Enumeration](../02-Enumeration/README.md) identified only SSH (22/tcp) and HTTP (80/tcp).

## Objectives

- Preserve a consistent methodology skeleton across engagements.
- Document the general approach that would be taken if an FTP service were discovered, without inventing findings that did not occur.

> [!WARNING]
> Nothing below reflects a real command run against this target. It documents general methodology only, per this repository's rule against fabricating commands, output, or vulnerabilities. See the [FTP cheat sheet](../../cheatsheets/ftp.md) for generic, tool-accurate reference syntax.

## Important Terminology

| Term | Definition |
|---|---|
| FTP | File Transfer Protocol — a plaintext (by default) protocol for file upload/download, typically on TCP/21 |
| Anonymous login | An FTP server configuration allowing login with the username `anonymous` and no meaningful password |
| Passive mode | An FTP mode where the client, not the server, initiates the data connection — often required through firewalls/NAT |

## Concepts Explained

When an FTP service is discovered during enumeration (typically via Nmap identifying `21/tcp open ftp`), the standard methodology is:

1. Identify the FTP daemon and version from the banner.
2. Attempt anonymous authentication.
3. If authentication succeeds, enumerate available files/directories and check both read and write permissions.
4. Cross-reference the daemon version against known CVEs only after confirming the version through direct interaction — never assume a version from a banner alone without corroboration.

## Commands and Syntax

No FTP commands were executed against this target. See [`cheatsheets/ftp.md`](../../cheatsheets/ftp.md) for general, accurate reference syntax to apply if an FTP service is discovered on a future target.

## Command Breakdown

Not applicable — no FTP commands were run against this target.

## Examples

Not applicable — see [`cheatsheets/ftp.md`](../../cheatsheets/ftp.md) for generic command examples.

## Screenshots

Not applicable — no FTP service was present to capture.

## Observations

- Not applicable — no FTP service was present on this target.

## Common Mistakes

- See [`cheatsheets/ftp.md`](../../cheatsheets/ftp.md) for common FTP enumeration mistakes to avoid in future engagements.

## Notes

This section will be updated with real, engagement-specific findings the next time an assessment in this knowledge base involves an FTP service.

## Additional Context

> [!NOTE]
> The following is supplementary background, not part of the original lab notes.

FTP (TCP/21) remains common on legacy or embedded systems. Even when not immediately exploitable, its presence is worth noting for a complete service inventory of a target.

## References

- [`cheatsheets/ftp.md`](../../cheatsheets/ftp.md)

## Key Takeaways

- Not every engagement touches every phase of a standard methodology — documenting "not applicable" honestly is more valuable than inventing findings to fill a template.
