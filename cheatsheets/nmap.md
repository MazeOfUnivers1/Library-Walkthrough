# Nmap Cheat Sheet

## Overview

Nmap ("Network Mapper") is a network scanning tool used for host discovery, port scanning, service/version detection, OS fingerprinting, and (via NSE) scripted vulnerability/enumeration checks.

> [!NOTE]
> This is a general-purpose reference sheet, not tied to a single engagement. Commands actually executed and their real output are documented in [`docs/02-Enumeration`](../docs/02-Enumeration/README.md).

## Common Syntax

```bash
nmap [scan type] [options] <target>
```

## Useful Commands

| Command | Description |
|---|---|
| `nmap <target>` | Basic scan of the top 1000 TCP ports |
| `nmap -p- <target>` | Scan all 65535 TCP ports |
| `nmap -sV <target>` | Service/version detection |
| `nmap -sC <target>` | Run default NSE scripts |
| `nmap -A <target>` | Aggressive scan: OS detection, version detection, default scripts, traceroute |
| `nmap -O <target>` | OS detection only |
| `nmap -sU <target>` | UDP port scan |
| `nmap -Pn <target>` | Skip host discovery (treat host as up) |
| `nmap --script=<category> <target>` | Run NSE scripts from a given category |
| `nmap -oA <basename> <target>` | Output in all formats (normal, XML, grepable) |

## Important Options

| Option | Description |
|---|---|
| `-p <ports>` | Specify port(s), e.g., `-p 22,80,443` or `-p1-1000` |
| `-T0`–`T5` | Timing templates from paranoid (`0`) to insane (`5`) |
| `-v` / `-vv` | Increase verbosity |
| `--top-ports <n>` | Scan the `n` most common ports |
| `-sS` | TCP SYN ("stealth") scan (requires root/appropriate privileges) |
| `-sT` | TCP connect scan (no special privileges required) |
| `-Pn` | Skip host discovery / treat all hosts as online |

## NSE Script Categories

| Category | Description |
|---|---|
| `auth` | Authentication-related checks |
| `default` | Scripts run automatically with `-sC` |
| `discovery` | Additional information gathering |
| `exploit` | Scripts that actively attempt to exploit vulnerabilities |
| `intrusive` | Scripts likely to affect the target's performance/security |
| `safe` | Scripts unlikely to crash or negatively affect the target |
| `vuln` | Scripts specifically checking for known vulnerabilities |

## Examples

```bash
# Fast aggressive scan, insane timing
nmap -A -T5 10.129.134.230

# Full TCP port sweep before deeper scanning
nmap -p- -T4 10.129.134.230

# Run all locally installed "exploit" category NSE scripts
nmap --script=exploit 10.129.134.230

# Save output in all formats for later parsing
nmap -A -oA target_scan 10.129.134.230
```

## Common Mistakes

- Using `-T5` against unstable or production networks — it can cause false negatives or trigger IDS/IPS alerting.
- Relying only on the default top-1000-port scan and missing services on non-standard ports; follow up with `-p-` when time allows.
- Treating `--script=vuln` or `--script=exploit` results as fully authoritative without manual verification.
- Forgetting `-Pn` against hosts that block ICMP but are still reachable on TCP/UDP.

## References

- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [Nmap NSE Documentation](https://nmap.org/book/nse.html)

## Key Takeaways

- `-A` is a fast way to get a broad first-pass picture of a target.
- Match your timing template (`-T`) to the network/target's tolerance for speed vs. stealth.
- NSE script categories let you scale from safe/passive checks to more intrusive, exploit-oriented ones — choose deliberately.
