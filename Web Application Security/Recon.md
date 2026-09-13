# Reconnaissance Checklist — Web Application Security

Purpose

A practical, prioritized reconnaissance checklist and toolkit for web application bug‑bounty assessments.
Goal: find domains, subdomains, endpoints, credentials/leaks, cloud assets, technologies, and public disclosures efficiently and reproducibly.

---

## Quick Start (high-level playbook)

1) Passive collection: certificates, public repos, search engines, Wayback.
2) Passive subdomain aggregation: crt.sh, CertSpotter, security lists, assetfinder, subdomain bruteforcing (low/no-noise).
3) Active enumeration: amass/subfinder + massdns for resolution — take care with rate limits and scope.
4) Endpoint discovery: Wayback/waybackurls, gau/gauplus, Burp spider for auth-required areas.
5) JS & API surface analysis: parse JS for endpoints, keys, or hidden parameters.
6) Cloud & storage checks: S3/GS buckets, cloud console exposures.
7) Triaging & fingerprinting: technology detection, WAF/CDN identification, prioritize targets for manual testing.

---

## Notes & rules of engagement

- Always confirm scope / targets before active testing.
- Check for DNS wildcard records to avoid false positives (see Wildcard check).
- Respect rate limits, robots.txt, and the program's allowed testing rules.
- Keep output organized and timestamped (CSV/JSON form).

---

## Wordlists

- Custom: CeWL (crawl & create wordlists)

  - Example: `cewl -m 4 -w dict.txt https://site.url`

- Collections:
  - JHaddix all.txt: https://gist.github.com/jhaddix/f64c97d0863a78454e44c2f7119c2a6a
  - SecLists raft-large-words.txt: https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-words.txt

- Local tips: combine common prefixes, services, and environment names; use altdns for permutations.

---

## Subdomain enumeration (passive → active)

**Pre-check: check for DNS wildcards:**

- `dig +short randomsubdomain.example.com`

- If responses exist for random names, treat brute-force results as suspect.

**Passive sources (low-noise)**

- crt.sh (certificates)
- Certspotter / Censys / Shodan
- GitHub/GitLab repository scraping (dorks)
- DNSDumpster, SecurityTrails, PassiveTotal

**Tools (suggested)**

- assetfinder: https://github.com/tomnomnom/assetfinder
- amass (passive + active): https://github.com/OWASP/Amass
  - Example: `amass enum -passive -d example.com -o amass_passive.txt`
- subfinder: https://github.com/ice3man543/subfinder
- dnsdumpster: https://dnsdumpster.com/
- ReconDog, Sublist3r, knock, ReconCat

**Active discovery & bruteforce**

- massdns (fast resolution), masscan for port scans
- altdns for permutations: `python3 altdns.py -i subdomains.txt -o domains.txt -w words.txt`
- subbrute, dnsrecon, dnssearch, dnsmap

**Example flow:**

1. Aggregate (crt.sh, amass, subfinder, assetfinder) → `subs.txt`
2. Permute with altdns → `permuted.txt`
3. Resolve with massdns → `resolved.txt`
4. Filter & scan open ports with nmap

---

## Domain-level correlation & related domains

- Reverse WHOIS: https://viewdns.info/reversewhois/, https://reverse.report/
- Reverse IP / Virtual host detection: https://www.yougetsignal.com/tools/web-sites-on-web-server/
- Domains on same nameserver: https://dns.coffee/nameservers
- DomLink (linking domains): https://github.com/vysecurity/DomLink
- Expired domains and takeover checks: domainhunter (https://github.com/threatexpress/domainhunter)

---

## Endpoint discovery (URLs, parameters, endpoints)

- Wayback Machine (archive.org) and waybackurls
- gau / gauplus (get all URLs) - great for archived endpoints and parameter discovery
  - Example: `echo example.com | gau --subs > gau_urls.txt`
- cc.py (content discovery) https://github.com/si9int/cc.py
- Web spidering (Burp, OWASP ZAP) for authenticated areas

---

## JavaScript & API surface analysis

- Collect and parse JS files (hidden endpoints, API keys, feature flags)
- Tools:
  - JSParser / LinkFinder / Subjs / gf + patterns
    - JSParser: https://github.com/nahamsec/JSParser
    - LinkFinder: https://github.com/GerbenJavado/LinkFinder
  - grep, ripgrep for quick patterns: e.g., `rg -n "apiKey|secret|token|endpoint|eval\("`

**Example JS workflow:**

1. Gather JS files via crawling, wayback, gau, and site scraping.
2. Run LinkFinder/JSParser to extract endpoints and parameters.
3. Feed endpoints into a wordlist for fuzzing (ffuf/gf).

---

## Site inspection & visual recon

- Visual screenshots and quick triage:
  - Aquatone: https://github.com/michenriksen/aquatone
  - EyeWitness (or Eyewitness alternatives) for screenshots and manual review
- Robots.txt, sitemap.xml checks
- Manual browsing for auth workflows, upload points, error messages
- Use Burp/BApps to capture client-side behavior and tokens

---

## Third-party surface & code repositories

- Check for third-party integrations: CDNs, payment providers, auth providers, 3rd-party scripts
- Search code repositories for secrets, endpoints, tokens:
  - GitHub dorks: https://github.com/techgaun/github-dorks
  - zen (email finder): https://github.com/s0md3v/zen
- Jenkins, Bitbucket, GitLab public projects and CI artifacts

---

## Cloud enumeration & storage brute force

- S3/Google Cloud/Azure storage checks:
  - bucket_finder (digi.ninja): https://digi.ninja/projects/bucket_finder.php
  - lazys3: https://github.com/nahamsec/lazys3
  - teh_s3_bucketeers: https://github.com/tomdev/teh_s3_bucketeers
  - Sandcastle / CloudScraper
- Manual checks: https://buckets.grayhatwarfare.com/ and direct URL patterns:
  - `[bucket].s3.amazonaws.com`
  - `[bucket].storage.googleapis.com`
- Look for misconfigured CORS or public write access

---

## Certificate & CT log enumeration

- crt.sh: https://crt.sh/
  - Example query: `https://crt.sh/?q=%25example.com`
- Certspotter (API): https://certspotter.com/api/v0/certs?domain=example.com
- Facebook CT monitoring: https://developers.facebook.com/tools/ct/
- Censys and VirusTotal for additional certs/info

---

## Technology profiling & fingerprinting

- BuiltWith: https://builtwith.com/
- Wappalyzer: https://www.wappalyzer.com/
- W3Techs / Netcraft / WhatCMS
- Headers & responses: `curl -I`, `httpx -v`, or Nmap http-enum scripts

---

## Public disclosures, leaks & intel

- HackerOne public disclosures: https://hackerone.com/hacktivity
- Open Bug Bounty: https://www.openbugbounty.org/
- Pastebin, GitHub/GitLab commits, Google dorks
- Search for public paste/data leaks and credential dumps

---

## Search engines & Shodan

- Google dorks for indexed admin panels, config files, backups
- Shodan for exposed services; search by org/IP/netblock for target

---

## Output, triage & automation

- Keep structured outputs: CSV/JSON for hosts, endpoints, JS artifacts.
- Tag assets by criticality: external API, auth-required, upload endpoint, admin panel.
- Use tools like `jq`, `csvkit` for processing.
- Save raw tool outputs in a timestamped archive for reproducibility.

---

## Common commands & examples

- Check wildcard: `dig +short random123456.example.com`
- Zone transfer attempt: `dig AXFR example.com @ns1.example.com`
- Amass passive: `amass enum -passive -d example.com -o amass_passive.txt`
- Subfinder: `subfinder -d example.com -o subfinder.txt`
- Massdns resolve: `massdns -r resolvers.txt -t A -o S -w resolved.txt candidate_subs.txt`
- Waybackurls: `echo example.com | waybackurls > wayback_urls.txt`
- Gauplus: `echo example.com | gauplus --providers wayback,archivedotorg > gau.txt`
- LinkFinder: `python3 linkfinder.py -i file.js -o cli`
- cewl: `cewl -m 4 -w dict.txt https://site.url`

---

## Tool installation notes (quick)

- amass: `go install github.com/OWASP/Amass/v3/...@latest`
- subfinder: `go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest`
- massdns: build from repo (C)
- gau/gauplus: `go install github.com/lc/gau/v2/cmd/gau@latest` or gauplus repo
- waybackurls: `go install github.com/tomnomnom/waybackurls@latest`
- cewl: `gem install cewl` or apt package
- LinkFinder / JSParser: Python tools; `pip install -r requirements.txt`

---

## Prioritization guidance

- High value first: exposed admin panels, auth bypass endpoints, file uploads, API keys in JS, exposed S3 buckets with public read/write.
- Medium: internal-only subdomains, legacy apps with known CVEs.
- Low: generic marketing pages, CDN-hosted static content (unless JS/API present).

---

## References & resources

- SecLists, JHaddix wordlists, OWASP Amass, ProjectDiscovery tools
- Bookmark crt.sh, Wayback, Censys, Shodan for quick queries

---

## Change log / author notes

- Enhanced: reorganized sections, added commands, prioritized playbook, JS/cloud focus, output & triage guidance.
