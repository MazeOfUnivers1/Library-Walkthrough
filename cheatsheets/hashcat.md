# Hashcat Cheat Sheet

## Overview

Hashcat is a GPU-accelerated password recovery tool used to crack captured or extracted password hashes offline, as opposed to online, network-based dictionary attacks (e.g., Hydra against a live login service).

> [!NOTE]
> This is a general-purpose reference sheet. Hashcat was not used in the engagement documented in this repository — the credential attack performed used Hydra against a live SSH service instead (see [`docs/05-Credentials`](../docs/05-Credentials/README.md)).

## Common Syntax

```bash
hashcat -m <hash-type> -a <attack-mode> <hash-file> <wordlist-or-mask>
```

## Useful Commands

| Command | Description |
|---|---|
| `hashcat -m 0 hashes.txt rockyou.txt` | Dictionary attack against raw MD5 hashes |
| `hashcat -m 1000 hashes.txt rockyou.txt` | Dictionary attack against NTLM hashes |
| `hashcat -m 1800 hashes.txt rockyou.txt` | Dictionary attack against sha512crypt (Linux `/etc/shadow`) hashes |
| `hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a` | Brute-force/mask attack, 6 characters, full charset |
| `hashcat --show hashes.txt` | Show already-cracked results for a hash file |

## Important Options

| Option | Description |
|---|---|
| `-m <n>` | Hash type/mode (e.g., `0` = MD5, `1000` = NTLM, `1800` = sha512crypt) |
| `-a <n>` | Attack mode: `0` = dictionary, `1` = combination, `3` = brute-force/mask, `6`/`7` = hybrid |
| `-r <rule-file>` | Apply a rule file to mutate wordlist entries |
| `-o <file>` | Output cracked results to a file |
| `--force` | Bypass certain hardware/OpenCL warnings (use with care) |
| `--username` | Indicates the hash file includes a `username:hash` format |

## Examples

```bash
# Crack MD5 hashes with rockyou.txt
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt

# Apply a common mutation rule set to the wordlist
hashcat -m 0 -a 0 -r /usr/share/hashcat/rules/best64.rule hashes.txt /usr/share/wordlists/rockyou.txt

# Mask attack: 8-character, all lowercase
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l?l?l?l?l
```

## Common Mistakes

- Selecting the wrong hash mode (`-m`) — hashcat will run but never find a match against mismatched hash formats.
- Forgetting `--username` when the hash file includes usernames, causing parsing failures.
- Using an unnecessarily large mask/brute-force keyspace when a dictionary + rules approach would be far faster.

## References

- [Hashcat documentation](https://hashcat.net/wiki/)
- [Hashcat example hash types](https://hashcat.net/wiki/doku.php?id=example_hashes)

## Key Takeaways

- Correctly identifying the hash type (`-m`) is the single most important step before cracking.
- Dictionary + rules attacks are usually far more time-efficient than pure brute force for real-world passwords.
- Offline cracking (Hashcat) and online brute forcing (Hydra) are complementary; use offline cracking whenever a hash can be captured/extracted, since it avoids network round-trips and target-side lockouts.
