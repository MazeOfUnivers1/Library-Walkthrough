# Cybersecurity Knowledge Base

> A personal, methodology-driven knowledge base documenting penetration testing lab notes — recon, enumeration, web assessment, credential attacks, initial access, and privilege escalation — captured from hands-on practice against an isolated training target.

![Markdown](https://img.shields.io/badge/docs-markdown-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![Made with](https://img.shields.io/badge/made%20with-Kali%20Linux-557C94)
![Contributions](https://img.shields.io/badge/contributions-welcome-orange)

---

## Table of Contents

- [About](#about)
- [Repository Structure](#repository-structure)
- [Learning Objectives](#learning-objectives)
- [Topics Covered](#topics-covered)
- [Navigation](#navigation)
- [Cheat Sheets](#cheat-sheets)
- [Tools Used](#tools-used)
- [Repository Statistics](#repository-statistics)
- [How to Use This Repository](#how-to-use-this-repository)
- [License](#license)
- [Acknowledgements](#acknowledgements)
- [Future Improvements](#future-improvements)

---

## About

This repository is **not** a step-by-step walkthrough or a spoiler guide. It is a structured knowledge base built from my own lab notes, terminal sessions, and screenshots, organized the way professional security references (HackTricks, PayloadsAllTheThings, OWASP, Microsoft Learn) organize their documentation — by **technique and methodology**, not by "answer."

Every command, flag, and observation documented here was actually executed against an isolated, authorized training target (a private lab VPN range, `10.129.134.230`). Nothing is fabricated. Where extra background is useful for understanding *why* a technique works, it is clearly separated into an **Additional Context** section so my own observations are never confused with supplementary theory.

> [!IMPORTANT]
> This repository documents activity performed only against systems I was explicitly authorized to test, inside an isolated training environment. None of the techniques here should be used against systems you do not own or do not have explicit written permission to test.

---

## Repository Structure

```
.
├── README.md                        # This file
├── LICENSE                          # MIT License
├── .gitignore
│
├── docs/
│   ├── 01-Recon/                    # Host discovery, connectivity checks
│   ├── 02-Enumeration/              # Nmap scanning & service enumeration
│   ├── 03-Web/                      # Web app enumeration, directory fuzzing, robots.txt
│   ├── 04-FTP/                      # FTP enumeration methodology (reference)
│   ├── 05-Credentials/              # Wordlist-based credential attacks (Hydra)
│   ├── 06-Initial-Access/           # SSH foothold
│   └── 07-Privilege-Escalation/     # sudo misconfiguration → root
│
├── cheatsheets/                     # Standalone, tool-focused reference sheets
│   ├── nmap.md
│   ├── ffuf.md
│   ├── ftp.md
│   ├── hashcat.md
│   ├── netcat.md
│   ├── linux-enumeration.md
│   ├── privilege-escalation.md
│   └── reverse-shells.md
│
├── images/                          # Renamed, descriptive screenshots referenced in docs/
│
└── resources/                       # External references and further reading
```

---

## Learning Objectives

- Build a repeatable, documented methodology for approaching an unknown Linux target.
- Practice translating raw terminal output into structured, reviewable notes.
- Understand common web misconfigurations (information disclosure via `robots.txt`, open directory listings, forms without CSRF protection).
- Practice credential-attack methodology against exposed services (SSH).
- Understand and document a real-world `sudo` privilege-escalation misconfiguration (NOPASSWD + wildcard binary path).

## Topics Covered

| Category | Description |
|---|---|
| Network Reconnaissance | Interface/VPN verification, host liveness checks |
| Service Enumeration | Nmap scanning (`-A`, `-T5`, NSE vulnerability/exploit scripts) |
| Web Enumeration | Directory/file fuzzing, `robots.txt` analysis, open directory indexing |
| Credential Attacks | Dictionary-based SSH brute forcing with Hydra |
| Initial Access | SSH authentication with recovered credentials |
| Privilege Escalation | Abusing a `sudo` NOPASSWD rule on a user-writable Python script |

## Navigation

| Section | Description |
|---|---|
| [01 - Recon](docs/01-Recon/README.md) | Establishing connectivity and confirming the target is reachable |
| [02 - Enumeration](docs/02-Enumeration/README.md) | Nmap service/version/vulnerability scanning |
| [03 - Web](docs/03-Web/README.md) | Web application enumeration and content discovery |
| [04 - FTP](docs/04-FTP/README.md) | FTP enumeration methodology (not present on this target — reference only) |
| [05 - Credentials](docs/05-Credentials/README.md) | Dictionary attack against SSH with Hydra |
| [06 - Initial Access](docs/06-Initial-Access/README.md) | Authenticating over SSH and locating the user flag |
| [07 - Privilege Escalation](docs/07-Privilege-Escalation/README.md) | Escalating from a low-privilege shell to root |

## Cheat Sheets

| Cheat Sheet | Description |
|---|---|
| [Nmap](cheatsheets/nmap.md) | Scanning syntax, timing templates, NSE script categories |
| [FFUF](cheatsheets/ffuf.md) | Web content/directory fuzzing syntax and matcher/filter options |
| [FTP](cheatsheets/ftp.md) | FTP enumeration and interaction reference |
| [Hashcat](cheatsheets/hashcat.md) | Hash-cracking mode reference |
| [Netcat](cheatsheets/netcat.md) | Listener/connection reference for shells and file transfer |
| [Linux Enumeration](cheatsheets/linux-enumeration.md) | Manual post-exploitation enumeration checklist |
| [Privilege Escalation](cheatsheets/privilege-escalation.md) | Common Linux privesc vectors, including `sudo` misconfigurations |
| [Reverse Shells](cheatsheets/reverse-shells.md) | Common one-liner reverse/interactive shell payloads |

## Tools Used

| Tool | Purpose |
|---|---|
| `nmap` | Port scanning, service/version detection, NSE scripts |
| `ffuf` | Web directory and content fuzzing |
| `hydra` | Dictionary-based credential attacks |
| `ssh` | Remote authentication and shell access |
| Firefox (Kali) | Manual web application review |
| `sudo` | Privilege escalation vector analysis |

## Repository Statistics

| Metric | Count |
|---|---|
| Documented phases | 7 |
| Cheat sheets | 8 |
| Screenshots referenced | 11 |
| Services enumerated | 2 (SSH, HTTP) |

## How to Use This Repository

1. Start at [01 - Recon](docs/01-Recon/README.md) and follow the numbered `docs/` folders in order — they mirror a realistic engagement methodology.
2. Use the `cheatsheets/` folder as quick, tool-specific reference material independent of any single engagement.
3. Every command block shows exactly what was run; every output block shows exactly what was observed. Nothing is invented.

## License

This repository is licensed under the [MIT License](LICENSE). Documentation content is provided for educational purposes only.

## Acknowledgements

- The security research community and documentation projects that inspired this repository's structure and style, including HackTricks, PayloadsAllTheThings, OWASP, and Microsoft Learn.
- The Stack Overflow community for reference material on Python interactive shell techniques (cited in [07 - Privilege Escalation](docs/07-Privilege-Escalation/README.md)).

## Future Improvements

- [ ] Add a dedicated Web Application Enumeration cheat sheet.
- [ ] Expand the Linux Enumeration cheat sheet with SUID/SGID and capability-hunting checklists.
- [ ] Add diagrams illustrating the overall attack chain.
- [ ] Backfill FTP documentation once an engagement involving FTP is completed.
