# Repository Expansion Roadmap & Backlog

**Version:** 1.0-alpha (First Commit)  
**Status:** Phase 1 Complete; Phase 2–3 Planned  
**Maintainer:** Community-driven (volunteer coordination)

---

## Current State (This Commit)

### ✅ Delivered

**Core Categories (6 files)**
- [x] `/core/foundations/` — OSINT methodology, verification, attribution confidence frameworks
- [x] `/core/dns/` — Passive DNS, DNS lookups, subdomain enumeration, DNSSEC validation
- [x] `/core/reputation-abuse-reporting/` — VirusTotal, AbuseIPDB, URLhaus, abuse.ch, blocklists

**Use-Case Playbooks (3 files)**
- [x] `/use-cases/soc/` — Alert triage, enrichment templates, false-positive detection
- [x] `/use-cases/dfir/` — Infrastructure mapping, timeline building, threat actor correlation
- [x] `/use-cases/phishing-brand/` — (Reference available; expansion pending)

**Supporting Files**
- [x] `/README.md` — Repository overview, architecture, quick-start
- [x] `/CONTRIBUTING.md` — Schema, submission process, quality standards
- [x] `/communities/README.md` — 20+ high-signal communities with validation guidance
- [x] `/indexes/free-only.md` — Free tools summary (no cost, no signup)

### ⏳ Partially Complete

**Use-Cases (Need Expansion)**
- [ ] `/use-cases/phishing-brand/` — Detailed takedown workflows (skeleton exists)
- [ ] `/use-cases/threat-hunting/` — Hypothesis-driven investigation patterns
- [ ] `/use-cases/cti/` — Campaign tracking, STIX/MISP production, sharing

**Indexes (Tier Coverage)**
- [ ] `/indexes/freemium.md` — Tools with free + paid tiers
- [ ] `/indexes/paid-commercial.md` — Enterprise platforms

### 🔜 Not Yet Started

See Phase 2 & 3 below.

---

## Phase 2: Depth & Completeness (Planned: Q1–Q2 2025)

### Core Categories to Deepen

**Priority 1 (Critical for SOC/DFIR Workflows)**

1. **`/core/domains-whois-rdap/`** (3–4 hours)
   - [ ] Expand WHOIS tools (WhoisXML, DomainTools, Spyse)
   - [ ] RDAP detailed examples (ARIN, RIPE, LACNIC, APNIC, AFRINIC)
   - [ ] Registrant correlation techniques
   - [ ] Privacy proxy detection (GDPR implications)
   - **Data source research needed:** Primary registrar APIs, ICANN whois protocol docs

2. **`/core/ip-intel-geo/`** (2–3 hours)
   - [ ] IP ownership lookup tools (MaxMind, IP2Location, IPQS)
   - [ ] Geolocation accuracy caveats
   - [ ] ASN → Organization mapping
   - [ ] Reverse IP hosting (shared hosting, cloud provider ranges)
   - **Data source research needed:** MaxMind GeoIP2 docs, cloud provider CIDR lists

3. **`/core/asn-bgp-routing/`** (2–3 hours)
   - [ ] Expand BGP tools (PeeringDB, RIPEstat, looking glasses)
   - [ ] RPKI and route security
   - [ ] Route hijack/leak monitoring (for incident context)
   - [ ] ASN → Threat actor correlation patterns
   - **Data source research needed:** RPKI monitoring tools, PeeringDB API

4. **`/core/certificates-ct-tls/`** (3–4 hours)
   - [ ] Expand CT log search tools (Censys, Splunk, Shodan)
   - [ ] Certificate clustering for campaign attribution
   - [ ] Self-signed cert fingerprinting
   - [ ] TLS fingerprinting context (defensive; not evasion)
   - [ ] SAN pivoting for subdomain discovery
   - **Data source research needed:** CT log specification (RFC 6962), certificate research papers

5. **`/core/hosting-cloud/`** (2–3 hours)
   - [ ] Cloud provider CIDR ranges (AWS, Azure, GCP, etc.)
   - [ ] Managed services identification (Wordpress.com, Shopify, etc.)
   - [ ] Reverse IP hosting (finding other sites on same IP)
   - [ ] Provider-specific infrastructure patterns
   - **Data source research needed:** AWS IP ranges docs, Azure ranges, GCP metadata

**Priority 2 (Specialized Workflows)**

6. **`/core/internet-wide-visibility/`** (3–4 hours)
   - [ ] Expand Censys, Shodan documentation with responsible-use emphasis
   - [ ] Safe-use guidelines for each platform
   - [ ] Rate limits and API costs
   - [ ] Legal implications by jurisdiction
   - [ ] Alternatives (GreyNoise, Shadowserver, passive sources)
   - **Data source research needed:** Censys/Shodan ToS, legal precedent on CFAA

7. **`/core/threat-intel/`** (4–5 hours)
   - [ ] STIX/MISP detailed format examples
   - [ ] IOC repositories (abuse.ch, MISP communities, commercial feeds)
   - [ ] Threat actor naming conventions (Unit 42 constellation, Dragos, etc.)
   - [ ] Malware family classification (Trojans, worms, ransomware, etc.)
   - [ ] Confidence scoring for CTI products
   - **Data source research needed:** STIX 2.1 spec, MISP docs, threat report taxonomy papers

### Use-Case Expansion

8. **`/use-cases/phishing-brand/`** (2–3 hours)
   - [ ] Lookalike domain detection patterns
   - [ ] Takedown workflow (registrar, hoster contacts)
   - [ ] Abuse report templates
   - [ ] Brand-risk memo format
   - [ ] Legal hold considerations

9. **`/use-cases/threat-hunting/`** (3–4 hours)
   - [ ] Hypothesis-driven investigation examples
   - [ ] Pivot patterns (beacon detection, C2 infrastructure hunting)
   - [ ] Detection rule writing (Suricata, Zeek example snippets)
   - [ ] Threat hunting tools (ThreatStream, Anomali, etc.)

10. **`/use-cases/cti/`** (3–4 hours)
    - [ ] Campaign clustering methodology
    - [ ] Confidence assessment (Admiralty system + Unit 42 framework)
    - [ ] STIX bundle generation examples
    - [ ] Threat feed evaluation (selecting which feeds to ingest)

### Additional Core Categories

11. **`/core/vpn-proxy-tor/`** (2–3 hours)
    - [ ] Exit node detection (Tor Project, I2P)
    - [ ] VPN/proxy detection signals (behavioral, infrastructure)
    - [ ] False-positive caveats (legitimate VPN use)

12. **`/core/web-fingerprinting-asm/`** (2–3 hours)
    - [ ] Tech stack fingerprinting (Wappalyzer, Shodan)
    - [ ] Favicon hashing
    - [ ] Metadata pivots (Google Analytics ID, Typekit fonts, etc.)
    - [ ] Historical snapshots (Wayback Machine, library.io)
    - [ ] ASM (Asset Security Management) workflow

13. **`/core/identifiers-pivots/`** (CRITICAL) (3–4 hours)
    - [ ] Pivot graph: Domain ↔ DNS ↔ IP ↔ ASN ↔ Org ↔ Cert ↔ Hosting ↔ Reputation ↔ TI
    - [ ] Worked examples for each pivot type
    - [ ] Confidence degradation as you traverse pivots
    - [ ] De-duplication across pivot paths

### Support Files

14. **`/indexes/freemium.md`** (1 hour)
    - [ ] Tools with free + paid tiers
    - [ ] Gating matrix (what's free vs. paid)

15. **`/indexes/paid-commercial.md`** (1 hour)
    - [ ] Enterprise platforms (DomainTools, SecurityTrails, GreyNoise, etc.)
    - [ ] Typical pricing/licensing models

**Phase 2 Effort Estimate:** 35–45 hours of research + writing

---

## Phase 3: Integration & Scaling (Planned: Q2–Q3 2025)

### Automation & Orchestration

1. **`/core/automation-pipelines/`** (4–5 hours)
   - [ ] Jupyter notebook examples (OSINT enrichment workflows)
   - [ ] OSINT frameworks (Maltego, Recon-ng, etc.)
   - [ ] SIEM integration (Splunk, ELK OSINT automation)
   - [ ] SOAR playbook examples (Demisto, Splunk SOAR)
   - [ ] Python scripts for bulk querying (VirusTotal API, SecurityTrails API, etc.)

2. **Detection Engineering Module** (3–4 hours)
   - [ ] Suricata rules for OSINT-discovered indicators
   - [ ] Zeek scripts for infrastructure detection
   - [ ] YARA rules for malware correlation

3. **`/use-cases/asm-external-attack-surface/`** (3–4 hours)
   - [ ] Defensive ASM for owned infrastructure
   - [ ] Shadow IT discovery
   - [ ] Exposure monitoring (credentials, data leaks)
   - [ ] API inventory and monitoring

### Datasets & Standards

4. **`/core/datasets-standards/`** (4–5 hours)
   - [ ] IANA IPv4/IPv6 allocation documents
   - [ ] Public Suffix List (PSL) usage
   - [ ] RIR policies and data retention
   - [ ] STIX/MISP/TAXII protocol specs
   - [ ] Common OSINT datasets (passive DNS, CT logs)

5. **Community Datasets** (2–3 hours)
   - [ ] Curated threat feeds (academic, commercial, community)
   - [ ] Malware databases (SAMPLES / MalwareBazaar)
   - [ ] Public IOC bundles (MISP communities)

### Case Studies & Worked Examples

6. **Incident Response Case Studies** (4–5 hours)
   - [ ] Real-world campaign attribution (anonymized)
   - [ ] End-to-end OSINT investigation walkthrough
   - [ ] Lessons learned + evasion techniques observed

### Community Contributions

7. **Community Translation & Localization** (Ongoing)
   - [ ] Spanish OSINT guide (partner with Latin American teams)
   - [ ] Simplified summaries for resource-constrained teams

**Phase 3 Effort Estimate:** 25–30 hours

---

## Low-Priority Backlog (Future Consideration)

- [ ] Mobile OSINT (APK analysis, app fingerprinting)
- [ ] Cryptocurrency/blockchain OSINT (NFT tracking, wallet analysis)
- [ ] Supply chain security (software provenance, dependencies)
- [ ] Counter-OSINT (detecting/evading OSINT observation)
- [ ] Darknet OSINT (Tor, I2P, market monitoring) — **DEFENSIVE CONTEXT ONLY**

---

## Research & Data Source Gaps

**Current Research Debt:**
- [ ] Deep dive into legal frameworks (CFAA, GDPR, UK/EU cyberlaw) + OSINT intersection
- [ ] Comprehensive comparison of passive DNS providers (coverage, lag, accuracy)
- [ ] Cost-benefit analysis of free vs. freemium vs. paid OSINT stacks
- [ ] Threat actor infrastructure preference patterns (registrar, ASN, hosting)
- [ ] False-positive rates for common SOC OSINT queries (benchmarking)

**Sources Needed:**
- [ ] Law enforcement OSINT guidance (IETF, NIST, other)
- [ ] Academic OSINT research (Cornell, MIT, Stanford papers)
- [ ] Vendor threat reports (Palo Alto, Mandiant, Dragos) for methodology
- [ ] Community feedback from r/OSINT, r/threatintel, SANS forums

---

## Community Contribution Priorities

**We need help with:**

1. **Tool verification:** Test tools listed; report accuracy of cost/API/features
2. **Use-case examples:** Submit redacted case studies from your investigations
3. **Community signal:** Recommend tools from your SOC/DFIR experience
4. **Translation:** Volunteer to translate core modules into your language
5. **Playbook refinement:** Feedback on SOC/DFIR playbooks from field experience

**To contribute:** See [/CONTRIBUTING.md](./) for submission process.

---

## Success Metrics (Quarterly Review)

- **Adoption:** GitHub stars, forks, contributor count
- **Community:** Activity in r/OSINT, r/threatintel mentions
- **Accuracy:** User-reported errors/outdated info rate
- **Completeness:** Coverage of "essential tools" (70%+ of critical category)
- **Maintenance:** Average PR merge time, response time to issues

---

## Release Schedule

- **v1.0 (This):** Core categories + 3 playbooks + foundations (DONE)
- **v1.1 (Mar 2025):** Phase 2 categories (domains, IP, ASN, hosting, threat-intel deepened)
- **v1.2 (Jun 2025):** Phase 3 automation, datasets, case studies
- **v2.0 (Dec 2025):** Community refinement, advanced use-cases, localization

---

## Maintenance Cadence

- **Weekly:** Monitor for dead links, tool updates
- **Monthly:** Community feedback incorporation, new tool vetting
- **Quarterly:** Major version review, roadmap adjustment
- **Annually:** Comprehensive audit, strategic planning

---

## Contact & Governance

**Current Maintainers:** [To be identified; seeking volunteers with 5+ years SOC/DFIR experience]

**How to propose changes:**
1. GitHub Issues for discussion
2. GitHub Discussions for broader community input
3. Quarterly community calls (Zoom/BigBlueButton)

---

**This repository is built by defenders, for defenders. Your contributions matter.**

Questions? Open an issue. Ideas? Start a discussion. Ready to contribute? See [/CONTRIBUTING.md](./).
