# Defensive OSINT Repository
## Network Infrastructure Attribution & Threat Intelligence Mapping for SOC/DFIR Teams

**Version:** 1.0-alpha  
**Last Updated:** December 2025  
**Maintainers:** Community-driven defensive security practitioners  

---

## ⚠️ Responsible Use Statement

This repository is designed exclusively for **lawful, defensive security operations**. All resources, methodologies, and tools listed serve to help defenders:

- **Attribute** threat infrastructure for incident response and threat hunting
- **Detect** malicious activity through passive reconnaissance
- **Report** abuse and facilitate takedown operations
- **Enrich** security investigations with contextual intelligence

**This is NOT a guide for:**
- Offensive reconnaissance or active scanning without authorization
- Exploitation, evasion, or stealth techniques
- Building or operating malicious infrastructure
- Unauthorized access to systems or data

**Legal Caveat:** Users are responsible for ensuring their activities comply with all applicable laws and regulations in their jurisdiction. Many OSINT activities exist in a legal gray zone; understand your local laws before acting on findings.

---

## Quick Start for SOC Teams

### 5-Minute Alert Enrichment Workflow

1. **Triage Alert** → Identify IoCs (IP, domain, email, hash)
2. **Quick Reputation Check** → Use [VirusTotal](#reputation-abuse-reporting) + [AbuseIPDB](#reputation-abuse-reporting)
3. **Infrastructure Pivot** → Cross-reference DNS/WHOIS via [SecurityTrails](#domains-whois-rdap) or [DomainTools](#domains-whois-rdap)
4. **Confidence Grade** → Correlate signals; document assumptions
5. **Take Action** → Alert analysis leads to detection tuning or incident escalation

**For deeper investigation:** See [/use-cases/soc](#use-cases-playbooks) playbook.

---

## Repository Architecture

### Two-Layer Design

**Layer A: Core Primitives** (`/core/`)  
A canonical library of resources organized by **data type** and **pivot capability**. Every tool appears once here, providing the single source of truth.

**Layer B: Use-Cases** (`/use-cases/`)  
Defensive workflows (playbooks) for SOC, DFIR, Phishing/Brand Protection, CTI, and Threat Hunting. These **cross-link to Core** without duplicating full entries.

---

## Core Categories

### Infrastructure Primitives

- **[Domains, WHOIS & RDAP](./core/domains-whois-rdap/)** — Domain registration, ownership history, registrant correlation
- **[DNS Intelligence](./core/dns/)** — Passive DNS, DNS records, nameserver analysis, DNSSEC
- **[IP Intelligence & Geolocation](./core/ip-intel-geo/)** — IP ownership, netblocks, geographic attribution
- **[ASN, BGP & Routing](./core/asn-bgp-routing/)** — Autonomous system analysis, prefix history, route security
- **[Hosting & Cloud Providers](./core/hosting-cloud/)** — Cloud infrastructure range identification, provider attribution
- **[CDN, WAF & Reverse Proxies](./core/cdn-waf-reverse-proxy/)** — Identifying reverse proxies, WAF detection, origin attribution limitations

### Threat Visibility & Detection

- **[VPN, Proxy & Tor Infrastructure](./core/vpn-proxy-tor/)** — Exit node detection, proxy identification, false-positive caveats
- **[Certificates & CT Logs](./core/certificates-ct-tls/)** — CT log search, SAN pivoting, cert reuse clustering
- **[Web Fingerprinting & ASM](./core/web-fingerprinting-asm/)** — Tech stack identification, metadata pivots, favicon hashing
- **[Internet-Wide Visibility Platforms](./core/internet-wide-visibility/)** — Censys, Shodan, and similar with **responsible-use guardrails**
- **[Reputation, Abuse & Reporting](./core/reputation-abuse-reporting/)** — Blocklists, phishing trackers, abuse report workflows

### Threat Intelligence & Enrichment

- **[Threat Intelligence Standards & Datasets](./core/threat-intel/)** — IOC repositories, STIX/MISP, malware families, C2 tracking (defensive context only)
- **[Automation & Enrichment Pipelines](./core/automation-pipelines/)** — Notebooks, frameworks, orchestration tools (no offensive code)
- **[OSINT Foundations & Methodology](./core/foundations/)** — Verification, evidence handling, legal/ethical guidance
- **[Pivot Graph & Attribution Methodology](./core/identifiers-pivots/)** — How to map relationships: domain ↔ DNS ↔ IP ↔ ASN ↔ org ↔ cert ↔ hosting ↔ reputation

---

## Use-Case Playbooks

### [SOC Alert Triage & Enrichment](./use-cases/soc/)
**When to use:** Real-time alert investigation, rapid enrichment, false-positive reduction  
**Inputs:** IP, domain, URL, email, file hash  
**Outputs:** Enriched alert, confidence grade, incident severity recommendation  
**Core tools:** Quick reputation, passive DNS, certificate search, Whois pivot

### [DFIR Incident Scoping & Attribution](./use-cases/dfir/)
**When to use:** Post-compromise forensics, timeline building, infrastructure correlation  
**Inputs:** Observed artifacts, log data, network captures  
**Outputs:** Infrastructure map, threat actor correlation, evidence-grade notes  
**Core tools:** Historical DNS, Whois pivots, ASN analysis, certificate clustering

### [Phishing & Brand Protection](./use-cases/phishing-brand/)
**When to use:** Lookalike domain discovery, phishing campaign tracking, takedown coordination  
**Inputs:** Domain, registrant patterns, hosting signals  
**Outputs:** Brand-risk memo, IOC bundle, takedown packet  
**Core tools:** Passive DNS, reverse Whois, DNS DMARC/SPF, cert transparency

### [Threat Hunting & Detection Engineering](./use-cases/threat-hunting/)
**When to use:** Proactive threat searching, hypothesis-driven investigation  
**Inputs:** IoC pivot points, behavioral hypothesis  
**Outputs:** Hunt leads, detection rules (Suricata/Zeek), enrichment pack  
**Core tools:** Historical DNS, ASN analysis, infrastructure clustering

### [Cyber Threat Intelligence (CTI) Operations](./use-cases/cti/)
**When to use:** Campaign tracking, actor attribution, intelligence product generation  
**Inputs:** Multiple observables, threat feeds, open-source data  
**Outputs:** Actor/campaign summary, STIX bundle, confidence scoring  
**Core tools:** All categories—feeds, standards, clustering, confidence frameworks

---

## High-Signal Communities

See [/communities/README.md](./communities/) for **25+ active forums, Slack groups, and curated-list hubs** with validation guidance and community signal caveats.

**Key communities include:**
- r/OSINT, r/threatintel, r/cybersecurity (Reddit)
- DigitalForensicsResearch.org, SANS Internet Storm Center
- Bellingcat, ECIR (European CERT), NarrativeWire
- Awesome-lists ecosystem (jivoi/awesome-osint, awesome-security, etc.)

---

## Cost-Based Indexing

### [Free Tier Tools](./indexes/free-only.md)
All essential tools with zero entry cost for SOC teams.

### [Freemium Tier Tools](./indexes/freemium.md)
Tools with free tiers that unlock premium features for serious deployments.

### [Paid & Commercial Tools](./indexes/paid-commercial.md)
Enterprise-grade platforms with advanced capabilities.

---

## Mandatory Resource Schema

Every resource in this repository follows this schema:

```
- **Name:**
  - **URL:** [official link]
  - **Category Path:** e.g., `core/dns/passive-dns`
  - **Best For:** [1-sentence use case]
  - **Access:** Web | API | Both | Unknown
  - **API Availability:** Yes | No | Limited | Unknown
  - **Signup Required:** Yes | No | Unknown
  - **Cost Tier:** Free | Freemium | Paid
  - **License Type:** OSS | Source-available | Proprietary | Unknown
  - **Commercial Use:** Allowed | Restricted | Unknown
  - **Data Sources:** [where data comes from, if known]
  - **Limitations:** [1 critical caveat]
  - **Update Cadence:** [if known; else Unknown]
  - **Confidence:** High | Medium | Low
```

---

## Deduplication & Cross-Linking Rules

✅ **Single Canonical Home:** Every resource appears once under its primary category.

✅ **Cross-Referencing:** If a tool fits multiple categories, add "See also →" pointers elsewhere (no duplication).

✅ **Use-Cases Link to Core:** Playbooks reference Core pages via links; only list top-pick pointers, not full entries.

✅ **Tier Indexes are Summary Only:** `free-only.md`, `freemium.md`, and `paid-commercial.md` each list: Name + URL + CategoryPath (no full schema repetition).

---

## Contributing & Maintenance

See [/CONTRIBUTING.md](./CONTRIBUTING.md) for:
- Resource submission template
- Quality checklist (transparency, ToS compliance, safety framing)
- Dead link handling
- Tagging conventions
- Review process

**Current Maintainers:** Community volunteers with SOC/DFIR experience  
**How to Contribute:** Open an issue or PR with resource details + schema.

---

## Expansion Roadmap (Next Phases)

### Phase 2 (Planned)
- [ ] Deepen `/core/automation-pipelines/` with Jupyter notebooks + OSINT frameworks
- [ ] Add `/use-cases/asm-external-attack-surface/` for defensive ASM workflows
- [ ] Expand `/core/threat-intel/` with MISP/STIX/TAXII integration guides
- [ ] Build `/core/datasets-standards/` with IANA registry, PSL, CIDRs, RIR references

### Phase 3 (Future)
- [ ] Add defensive detection engineering playbooks (Suricata, Zeek)
- [ ] Incident response runbooks for common attack patterns
- [ ] Confidence scoring frameworks for SOC triage
- [ ] Integration guides for SIEM/SOAR platforms

---

## Quick Links

| Resource | Purpose |
|----------|---------|
| [Core Library](./core/) | Canonical tool & data source reference |
| [Use-Case Playbooks](./use-cases/) | SOC, DFIR, Threat Hunting workflows |
| [Communities](./communities/) | Forums, Slack groups, curated lists |
| [Contributing](./CONTRIBUTING.md) | Schema, quality standards, submission guide |
| [Cost Indexes](./indexes/) | Free, Freemium, Paid tool tiers |
| [Foundations](./core/foundations/) | OSINT methodology & legal/ethical guidance |

---

## License & Attribution

This repository is provided as a community resource for defensive security professionals. All external tools retain their original licenses. This documentation is provided under a permissive license to encourage community contribution and adaptation.

**Attribution:** Built on research from academic sources, NIST frameworks (DFIR), Unit 42 attribution methodology, and collective SOC/DFIR practitioner experience.

---

## Disclaimer

This repository is maintained on a best-effort basis. Tools, URLs, and services change frequently. Always verify current tool documentation, ToS, and legal status before deployment. Users assume all responsibility for lawful use.

---

**Next:** [Browse the Core Library →](./core/)  
**Or:** [Jump to SOC Playbook →](./use-cases/soc/)
