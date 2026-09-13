# Reconnaissance Checklist — Web Application Security

> A practical, prioritized reconnaissance playbook for web application bug bounty assessments.
>
> Goal: discover domains, subdomains, endpoints, credentials/leaks, cloud assets, technologies, and public disclosures efficiently and reproducibly.

---

## 0) Rules of Engagement (Read First)

- Always confirm scope before active testing.
- Respect program policy, rate limits, and allowed testing techniques.
- Check wildcard DNS behavior before brute force to reduce false positives.
- Keep results timestamped and reproducible (`CSV`/`JSON` + raw logs).

---

## 1) Quick Start Workflow (High-Level)

1. **Passive collection**: CT logs, public repos, search engines, Wayback.
2. **Subdomain aggregation**: crt.sh, CertSpotter, security sources, assetfinder.
3. **Active enumeration**: amass/subfinder + massdns resolution (scope-aware, rate-limited).
4. **Endpoint discovery**: Wayback/waybackurls, gau/gauplus, authenticated spidering.
5. **JS/API analysis**: parse JavaScript for endpoints, secrets, hidden params.
6. **Cloud/storage checks**: S3/Google Cloud/Azure buckets and misconfigurations.
7. **Fingerprinting & triage**: stack detection, WAF/CDN hints, attack-surface ranking.

---

## 2) Target Mapping

### Top-level target

- `target.com`
  - Enumerate known IP ranges / host IPs.
  - Track DNS records and related infrastructure.

---

## 3) Wordlists

### Custom wordlists

- **CeWL**
  - Build target-specific words from crawlable content.
  - Example:

```bash
cewl -m 4 -w dict.txt https://site.url
```

### Public wordlists

- **JHaddix all.txt**
  - https://gist.github.com/jhaddix/f64c97d0863a78454e44c2f7119c2a6a
- **SecLists raft-large-words.txt**
  - https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-words.txt

### Local tuning tips

- Combine environment patterns: `dev`, `stg`, `uat`, `prod`, `api`, `admin`, `internal`.
- Add brand/service terms and regional codes.
- Use **altdns** permutations to expand seed lists.

---

## 4) Subdomain Enumeration (Passive → Active)

### 4.1 Wildcard pre-check (critical)

```bash
dig +short randomsubdomain.example.com
```

- If random labels resolve, treat brute-force positives as potentially invalid.

### 4.2 Passive sources (low-noise)

- crt.sh (certificate transparency)
- CertSpotter / Censys / Shodan
- GitHub/GitLab code and commit dorks
- DNSDumpster / SecurityTrails / PassiveTotal

### 4.3 Tools

- Knock — https://github.com/guelfoweb/knock
- Sublist3r — https://github.com/aboul3la/Sublist3r
- LazyRecon — https://github.com/capt-meelo/LazyRecon
- subfinder — https://github.com/ice3man543/subfinder
- amass — https://github.com/OWASP/Amass
- ReconDog — https://github.com/s0md3v/ReconDog
- subbrute — https://github.com/TheRook/subbrute
- cloudflare_enum — https://github.com/mandatoryprogrammer/cloudflare_enum
- dnsmap — https://github.com/makefu/dnsmap
- altdns — https://github.com/infosec-au/altdns
- dnsrecon — https://github.com/darkoperator/dnsrecon
- dnssearch — https://github.com/evilsocket/dnssearch
- fierce — https://github.com/mschwager/fierce
- second-order — https://github.com/mhmdiaa/second-order
- assetfinder — https://github.com/tomnomnom/assetfinder
- DNSDumpster — https://dnsdumpster.com/
- massdns
- nmap

### 4.4 Active discovery and validation

- Permutation generation with altdns
- Fast resolution with massdns
- Port/service validation with nmap

### 4.5 Example flow

1. Aggregate from passive tools → `subs.txt`
2. Permute candidates with altdns → `permuted.txt`
3. Resolve with massdns → `resolved.txt`
4. Validate and prioritize with nmap/http probing

---

## 5) Domain Correlation & Related Assets

### Reverse WHOIS

- https://viewdns.info/reversewhois/
- https://reverse.report/

### Reverse IP / virtual host mapping

- https://www.yougetsignal.com/tools/web-sites-on-web-server/
- Reverse DNS via nmap

### Shared infrastructure pivots

- Same nameserver pivot: https://dns.coffee/nameservers
- Related domains via link analysis:
  - DomLink — https://github.com/vysecurity/DomLink

### Expired/legacy domain risks

- domainhunter — https://github.com/threatexpress/domainhunter

---

## 6) Endpoint Discovery (URLs, Params, APIs)

- Wayback Machine — https://archive.org/web/
- waybackurls
- gau / gauplus
  - Example:

```bash
echo example.com | gau --subs > gau_urls.txt
```

- cc.py — https://github.com/si9int/cc.py
- Authenticated crawling (Burp, OWASP ZAP)

---

## 7) JavaScript & API Surface Analysis

### Goals

- Discover hidden endpoints, parameters, feature flags, hardcoded secrets/tokens.

### Tools

- JSParser — https://github.com/nahamsec/JSParser
- LinkFinder — https://github.com/GerbenJavado/LinkFinder
- grep/ripgrep quick triage:

```bash
rg -n "apiKey|secret|token|endpoint|eval\("
```

### Example workflow

1. Gather JS via crawl + archives.
2. Extract endpoints/params with LinkFinder/JSParser.
3. Feed results into fuzzing lists (`ffuf`, `gf` patterns).

---

## 8) Site Inspection & Visual Recon

- Aquatone — https://github.com/michenriksen/aquatone
- EyeWitness
- `robots.txt`
- `sitemap.xml`
- Manual browsing for:
  - auth flows
  - uploads
  - error handling
  - token behavior

---

## 9) Third-Party Surface

### External dependencies

- Applications and storefront components
- CDNs
- Payment systems
- WAF providers
- Third-party scripts/CSS/JS

### Code repositories and CI exposure

- GitHub dorks — https://github.com/techgaun/github-dorks
- zen (email finder) — https://github.com/s0md3v/zen
- GitLab / Bitbucket / Jenkins public artifacts

---

## 10) Cloud Enumeration & Storage Brute Force

### Tools

- bucket_finder — https://digi.ninja/projects/bucket_finder.php
- lazys3 — https://github.com/nahamsec/lazys3
- teh_s3_bucketeers — https://github.com/tomdev/teh_s3_bucketeers
- Sandcastle — https://github.com/0xSearches/sandcastle
- CloudScraper — https://github.com/jordanpotti/CloudScraper

### Public bucket checks

- https://buckets.grayhatwarfare.com/
- Manual patterns:
  - `[bucket].s3.amazonaws.com`
  - `[bucket].storage.googleapis.com`

### Misconfiguration checks

- Public read/write
- CORS misconfiguration
- Directory listing/object leakage

---

## 11) Certificate & CT Log Enumeration

- crt.sh — https://crt.sh/
  - Example query: `https://crt.sh/?q=%25example.com`
- Facebook CT monitoring — https://developers.facebook.com/tools/ct/
- Certspotter API — `https://certspotter.com/api/v0/certs?domain=`
- Censys / VirusTotal certificate pivots

---

## 12) Technology Profiling

- BuiltWith — https://builtwith.com/
- Wappalyzer — https://www.wappalyzer.com/
- W3Techs — https://w3techs.com/sites
- Netcraft — https://sitereport.netcraft.com/
- WhatCMS — https://whatcms.org/
- Header/service fingerprinting:

```bash
curl -I https://target.tld
```

---

## 13) Public Disclosures, Leaks & Intel

- Open Bug Bounty — https://www.openbugbounty.org/
- HackerOne Hacktivity — https://hackerone.com/hacktivity
- Pastebin
- Google dorks
- Public commits and exposed credentials history

---

## 14) Search Engines & Internet-wide Recon

- Google Dorks (admin panels, backups, exposed files)
- Shodan (`org`, `net`, service fingerprints)

---

## 15) Brute-Force Files & Directories

- gobuster — https://github.com/OJ/gobuster
- dirsearch — https://github.com/maurosoria/dirsearch
- DirBuster — https://tools.kali.org/web-applications/dirbuster
- snallygaster — https://github.com/hannob/snallygaster
- fdb (File Disclosure Browser) — https://digi.ninja/projects/fdb.php

---

## 16) Google Hacking Database

- Exploit-DB / GHDB — https://www.exploit-db.com/

---

## 17) Metadata Gathering

- FOCA — https://github.com/ElevenPaths/FOCA
- strings
- recon-ng (metacrawler) — https://bitbucket.org/LaNMaSteR53/recon-ng
- ExifTool

---

## 18) Social Media & People Surface

- Facebook
- Twitter/X
- LinkedIn

---

## 19) Email Recon

### Gather addresses

- theHarvester — https://github.com/laramies/theHarvester
- Maltego — https://www.maltego.com/products/
- Samurai — https://github.com/OffXec/Samurai
- InSpy (LinkedIn enum) — https://github.com/leapsecurity/InSpy

### Verify addresses / breach context

- Hunter.io — https://hunter.io/email-finder
- haveibeenpwned — https://haveibeenpwned.com/
- Facebook/OSINT cross-verification

---

## 20) Useful Commands (Cheat Sheet)

```bash
# Wildcard check
dig +short random123456.example.com

# Zone transfer attempt
dig AXFR example.com @ns1.example.com

# Amass passive
amass enum -passive -d example.com -o amass_passive.txt

# Subfinder
subfinder -d example.com -o subfinder.txt

# Massdns resolve
massdns -r resolvers.txt -t A -o S -w resolved.txt candidate_subs.txt

# Waybackurls
echo example.com | waybackurls > wayback_urls.txt

# Gauplus
echo example.com | gauplus --providers wayback,archivedotorg > gau.txt

# LinkFinder
python3 linkfinder.py -i file.js -o cli

# CeWL
cewl -m 4 -w dict.txt https://site.url
```

---

## 21) Tool Install Notes (Quick)

- amass

```bash
go install github.com/OWASP/Amass/v3/...@latest
```

- subfinder

```bash
go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

- gau

```bash
go install github.com/lc/gau/v2/cmd/gau@latest
```

- waybackurls

```bash
go install github.com/tomnomnom/waybackurls@latest
```

- cewl

```bash
gem install cewl
```

- massdns: build from source (C project)
- LinkFinder/JSParser: Python toolchain (`pip` requirements)

---

## 22) Prioritization Model

### High-value targets first

- Admin panels
- Auth/authz bypass candidates
- Upload endpoints
- JS-exposed keys/tokens
- Public-write cloud buckets

### Medium

- Internal subdomains with partial exposure
- Legacy services with known-CVE fingerprints

### Low

- Static brochure/marketing pages (unless JS/API surface exists)

---

## 23) Output, Triage & Automation

- Save structured outputs: hosts, endpoints, JS findings, cloud assets.
- Tag by criticality (auth-required, upload, admin, external API).
- Use `jq`, `csvkit`, and timeline snapshots for reproducibility.
- Archive raw artifacts per run.

---

## 24) References

- SecLists
- JHaddix wordlists
- OWASP Amass
- ProjectDiscovery ecosystem
- crt.sh / Wayback / Censys / Shodan

---

## Change Log

- Reworked into a Notion-style, structured playbook.
- Merged original checklist + expanded recon inventory.
- Added grouped sections, command snippets, and prioritization flow.
