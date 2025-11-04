# checklink

A quick URL triage tool for checking suspicious links.

## Important Privacy Warning

This tool performs real network requests that may reveal investigated URLs to third parties. See the warning section in the script header for details.

## Overview

checklink v0.4.3 is a POSIX-compliant shell script that surfaces obvious red flags in URLs. It is **not** a comprehensive security tool but rather a quick first-pass checker that provides context to help you decide whether a link looks suspicious.

## Features

- VirusTotal verdict (lookup-only by default)
- Domain registration age (RDAP / .gr WHOIS)
- Certificate age and issuer analysis
- URL heuristics (scheme, subdomains, TLD, punycode, userinfo, IP literals)
- DNS resolution with ASN lookup

## Installation

### Prerequisites

Required:
- `curl`, `jq`, `openssl`
- [VirusTotal CLI](https://github.com/VirusTotal/vt-cli)

Optional:
- `whois` (for .gr domains)
- `dig` (for DNS checks)

### Quick Install

Install to ~/bin (no sudo required):
```bash
mkdir -p ~/bin
curl -L https://raw.githubusercontent.com/silohunt/checklink/main/checklink -o ~/bin/checklink && chmod +x ~/bin/checklink
```

Make sure `~/bin` is in your PATH. Add to your `~/.bashrc` or `~/.zshrc` if needed:
```bash
export PATH="$HOME/bin:$PATH"
```

### Full Setup

1. Install the VirusTotal CLI and configure your API key:
```bash
   vt init YOUR_API_KEY
```

2. Download the script:
```bash
   curl -O https://raw.githubusercontent.com/silohunt/checklink/main/checklink
   chmod +x checklink
```

3. Move to system PATH (optional):
```bash
   sudo mv checklink /usr/local/bin/checklink
```

## Usage
```bash
checklink [-q] [-u] [--no-age] [--version] <url-or-domain>
```

### Examples
```bash
# Basic check
checklink https://suspicious-site.com

# Quiet mode (summary only)
checklink -q domain.gr

# Upload to VirusTotal for fresh analysis
checklink -u suspicious-url.com

# Skip domain/cert age checks
checklink --no-age bit.ly/abc123
```

## How to Interpret Results

- **Treat results as risk indicators only**
- Clear red flags (VT "malicious," IP-literal hosts, very fresh cert on mature domain) warrant caution
- "No detections" with warnings suggests verifying via another channel
- If services are unavailable, treat output as incomplete, not as "safe"

## Limitations

- Relies on public APIs that may be rate-limited or unavailable
- Uses intentionally minimal heuristics to avoid false positives
- Cannot replace proper security tools or expert analysis
- Not suitable for confidential analysis (makes external queries)

## License

MIT License - see script header for details

## Author

Fanis Sklinos with Claude Code and ChatGPT (2025)
