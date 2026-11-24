# FFUF Cheat Sheet

## Overview

`ffuf` ("Fuzz Faster U Fool") is a fast web fuzzing tool used for directory/file discovery, virtual host discovery, parameter fuzzing, and more.

> [!NOTE]
> This is a general-purpose reference sheet. The real command and output actually used in this repository's lab notes are documented in [`docs/03-Web`](../docs/03-Web/README.md).

## Common Syntax

```bash
ffuf -w <wordlist> -u <url-with-FUZZ-keyword> [options]
```

## Useful Commands

| Command | Description |
|---|---|
| `ffuf -w wordlist.txt -u http://target/FUZZ` | Basic directory/file fuzzing |
| `ffuf -w wordlist.txt -u http://FUZZ.target.com` | Virtual host / subdomain fuzzing |
| `ffuf -w wordlist.txt -u http://target/FUZZ.php` | Fuzz with a fixed file extension |
| `ffuf -w users.txt:USER -w passwords.txt:PASS -u http://target/login -d 'user=USER&pass=PASS'` | Multi-wordlist fuzzing (e.g., login forms) |

## Important Options

| Option | Description |
|---|---|
| `-w <path>` | Wordlist path (optionally `path:KEYWORD` for multiple wordlists) |
| `-u <url>` | Target URL; `FUZZ` marks the injection point |
| `-X <method>` | HTTP method (default `GET`) |
| `-H <header>` | Add a custom header (repeatable) |
| `-d <data>` | POST body data |
| `-mc <codes>` | Match responses by status code(s) |
| `-fc <codes>` | Filter out responses by status code(s) |
| `-fs <size>` | Filter out responses by size |
| `-fw <words>` | Filter out responses by word count |
| `-t <n>` | Number of concurrent threads |
| `-recursion` | Recurse into discovered directories |
| `-o <file>` | Write results to a file |

## Examples

```bash
# Basic content discovery
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.129.134.230/FUZZ

# Filter out noisy 200 responses of a known size (e.g., a soft-404 page)
ffuf -w wordlist.txt -u http://target/FUZZ -fs 1234

# Fuzz for PHP files specifically
ffuf -w wordlist.txt -u http://target/FUZZ.php -mc 200,301,302,403
```

## Common Mistakes

- Not filtering out a consistent "soft 404" response size/word count, resulting in a wall of false-positive hits.
- Using a wordlist mismatched to the target's technology stack (e.g., a generic list against a known CMS with its own common paths).
- Forgetting `-recursion` when a discovered directory likely contains further unlisted subdirectories.
- Running with too many threads (`-t`) against a fragile target, causing instability or missed responses.

## References

- [ffuf GitHub repository](https://github.com/ffuf/ffuf)

## Key Takeaways

- Always follow up automated fuzzing hits with manual review — a status code alone doesn't tell you what's actually there.
- Tune matchers/filters (`-mc`, `-fc`, `-fs`, `-fw`) to cut through noise rather than scanning blind.
