# Core: Reputation, Abuse & Reporting

**Primary Data Type:** Blocklists, abuse reports, phishing databases, malware samples, threat signals  
**Primary Pivots:** IP/Domain → Reputation Score, Email → Phishing Reports, Hash → Malware Family  
**Common Use:** Quick triage, false-positive filtering, incident context, takedown workflows

---

## Overview

Reputation databases aggregate community and vendor reports of malicious activity, enabling rapid assessment of:

- **IP reputation:** Brute-force, spam, malware hosting, DDoS sources
- **Domain reputation:** Phishing, malware distribution, brand spoofing
- **File/URL reputation:** Known malware, exploit kits, phishing payloads
- **Email reputation:** Spam, phishing campaigns, compromised accounts

### Key Caveats

- **False Positives Abound:** A single malware vendor flagging an IP does not prove maliciousness; correlation matters
- **Reputation Lag:** Newly registered phishing domains may not appear in databases for 24–48 hours
- **Legitimate Services Flagged:** VPNs, proxies, and CDNs may appear "suspicious" due to aggregated user behavior
- **Report Quality Varies:** Community-sourced reports (Reddit, forums) require additional verification
- **Takedown Speed:** Some registrars/hosters ignore abuse reports; persistence required

---

## Canonical Tools & Resources

### Multi-Engine Threat Intelligence & File Analysis

- **VirusTotal**
  - **URL:** https://www.virustotal.com
  - **Category Path:** `core/reputation-abuse-reporting/multi-engine`
  - **Best For:** Rapid file/URL/domain/IP reputation; correlates 70+ antivirus engines; reveals relationships between IOCs
  - **Access:** Web + API
  - **API Availability:** Yes (free tier: 4 requests/min; paid: higher limits)
  - **Signup Required:** Yes (free account available)
  - **Cost Tier:** Freemium (free for personal use; enterprise licensing available)
  - **License Type:** Proprietary
  - **Commercial Use:** Restricted on free tier; allowed with commercial license
  - **Data Sources:** Antivirus engines, threat feeds, user submissions, automated sandbox analysis
  - **Limitations:** Single malware vendor flagging ≠ certainty (many false positives); VT relationships show correlation, not causation
  - **Update Cadence:** Real-time (for submissions); engines update continuously
  - **Confidence:** High (for aggregated consensus); Medium (for single-engine flags)

  **SOC Quick Tips:**
  - Flag count threshold: 5+ unrelated AV engines minimum before escalation
  - Check "Community Score" tab for user-reported context
  - Use "Relationships" tab to map infrastructure connections
  - Note submission timestamp vs. first-submission-time for aging

- **URLhaus** (by abuse.ch)
  - **URL:** https://urlhaus.abuse.ch
  - **Category Path:** `core/reputation-abuse-reporting/phishing-malware-urls`
  - **Best For:** Malware distribution URLs, phishing infrastructure, botnet C2 tracking
  - **Access:** Web + API
  - **API Availability:** Yes (free)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** OSS/Community
  - **Commercial Use:** Allowed (community project)
  - **Data Sources:** Community submissions, malware research, SIRT feeds
  - **Limitations:** Community-driven; not all malicious URLs reported; regional/language bias
  - **Update Cadence:** Real-time (as submissions received)
  - **Confidence:** Medium-High (curated by abuse.ch, but relies on community verification)

  **SOC Quick Tips:**
  - Filter by threat type (phishing, malware, exploit kit)
  - Download blocklist (RPZ format) for DNS-level blocking
  - Cross-reference with URLhaus ASN/hosting to identify campaigns

- **PhishTank** (by OpenDNS)
  - **URL:** https://www.phishtank.com
  - **Category Path:** `core/reputation-abuse-reporting/phishing`
  - **Best For:** Phishing URL detection, phishing campaign tracking, brand protection
  - **Access:** Web + API
  - **API Availability:** Yes (free, registration required)
  - **Signup Required:** Yes (free account)
  - **Cost Tier:** Free
  - **License Type:** Proprietary (free access)
  - **Commercial Use:** Allowed (check latest ToS)
  - **Data Sources:** Community submissions, verified by volunteers
  - **Limitations:** Verification lag; reported URLs may be taken down quickly, making them unavailable for analysis
  - **Update Cadence:** Real-time (verified submissions)
  - **Confidence:** Medium-High (community vetted, but quality varies)

### IP Reputation & Blocklists

- **AbuseIPDB**
  - **URL:** https://www.abuseipdb.com
  - **Category Path:** `core/reputation-abuse-reporting/ip-reputation`
  - **Best For:** IP abuse reporting, crowdsourced blacklist, brute-force/spam/DDoS source identification
  - **Access:** Web + API
  - **API Availability:** Yes (free tier: 1000 requests/day; paid: higher)
  - **Signup Required:** Yes
  - **Cost Tier:** Freemium
  - **License Type:** Proprietary
  - **Commercial Use:** Allowed (check tier)
  - **Data Sources:** Community abuse reports (categorized: brute-force, proxy, spam, etc.)
  - **Limitations:** Lower signal-to-noise than automated sources; sybil attacks possible; false positives from shared hosting
  - **Update Cadence:** Real-time (as reports submitted)
  - **Confidence:** Medium (high report count increases confidence; single report unreliable)

  **SOC Quick Tips:**
  - Abuse score >75% + 20+ reports → High confidence
  - Filter by abuse type (e.g., "SSH Brute Force" vs. "VPN IP")
  - Cross-check with BGPView for ASN/hosting context

- **Project Honey Pot** (HTTP:BL)
  - **URL:** https://www.projecthoneypot.org
  - **Category Path:** `core/reputation-abuse-reporting/ip-reputation`
  - **Best For:** Harvester/spammer detection, email harvesting intelligence
  - **Access:** Web + API (HTTP:BL)
  - **API Availability:** Yes (free registration)
  - **Signup Required:** Yes
  - **Cost Tier:** Free
  - **License Type:** Proprietary (free access)
  - **Commercial Use:** Allowed
  - **Data Sources:** Honeypot traps, distributed sensors
  - **Limitations:** Lower activity than commercial sources; long update cycles
  - **Update Cadence:** Daily
  - **Confidence:** Medium (good for email harvester identification)

### Malware & C2 Intelligence

- **abuse.ch (Malware Hash Registry, SSL Abuse Database)**
  - **URL:** https://abuse.ch
  - **Category Path:** `core/reputation-abuse-reporting/malware-c2`
  - **Best For:** Malware samples, C2 IP/domain identification, SSL certificate abuse detection
  - **Access:** Web + API (multiple databases)
  - **API Availability:** Yes (free)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** OSS
  - **Commercial Use:** Allowed
  - **Data Sources:** Security researchers, sandboxes, incident response sharing
  - **Limitations:** Dependent on researcher submissions; gaps in regional threats
  - **Update Cadence:** Real-time
  - **Confidence:** High—curated by respected security researchers

  **Key databases:**
  - **MalwareBazaar:** Malware samples (searchable by hash, family)
  - **URLhaus:** Malware distribution URLs (see above)
  - **SSL Abuse Database:** Malicious certificates and C2 infrastructure

- **Ransomware Track & Trace (RT&T)**
  - **URL:** https://ransomwaretracker.abuse.ch
  - **Category Path:** `core/reputation-abuse-reporting/ransomware`
  - **Best For:** Ransomware C2 tracking, operational security research (defensive)
  - **Access:** Web + API
  - **API Availability:** Yes (free)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** OSS
  - **Commercial Use:** Allowed (defensive only)
  - **Data Sources:** Security researchers, threat reports, takedown notices
  - **Limitations:** Coverage focuses on tracked variants; emerging ransomware may not appear
  - **Update Cadence:** Daily to real-time
  - **Confidence:** High—curated by domain experts

### Public Blocklists & DNSBL

- **Spamhaus DROP / EDROP** (Botnet C&C, Malware, Phishing)
  - **URL:** https://www.spamhaus.org
  - **Category Path:** `core/reputation-abuse-reporting/blocklists`
  - **Best For:** Botnet C&C, malware hosting, phishing infrastructure; DNS blacklist deployment
  - **Access:** Web + DNS RBL + RPZ feeds
  - **API Availability:** Limited (DNS-based queries)
  - **Signup Required:** No (for public feeds)
  - **Cost Tier:** Free (public; premium tiers for commercial)
  - **License Type:** Proprietary
  - **Commercial Use:** Restricted on public tier; commercial licenses available
  - **Data Sources:** Automated threat detection, law enforcement, abuse reports
  - **Limitations:** Undergoes periodic false positive audits but still has low false-positive rate
  - **Update Cadence:** Real-time
  - **Confidence:** High—gold standard for blocklists; low false-positive rate

- **SURBL & Multi-RBL** (Email Reputation)
  - **URL:** https://www.surbl.org
  - **Category Path:** `core/reputation-abuse-reporting/email-reputation`
  - **Best For:** Email gateway reputation, phishing URL detection, spam filtering
  - **Access:** DNS RBL
  - **API Availability:** DNS-based queries only
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** Proprietary (free access)
  - **Commercial Use:** Allowed (check terms)
  - **Data Sources:** Email spam traps, user reports, automated sensors
  - **Limitations:** Designed for email filtering; may flag legitimate URLs if widely distributed
  - **Update Cadence:** Real-time to hourly
  - **Confidence:** Medium-High (mature, widely deployed)

---

## Abuse Reporting Workflows

### Reporting Malicious Infrastructure (Defensive Action)

#### Step 1: Identify Responsible Parties

| Resource | Responsible Party | How to Find |
|----------|------------------|------------|
| **Domain** | Registrar, Hosting Provider | WHOIS (registrar field) + hosting IP lookup |
| **IP Address** | Hosting Provider, ISP | ARIN RDAP, RIPEstat, etc. |
| **Malware** | Hosting Provider (removes samples) | abuse@[hosting domain] or security@[domain] |
| **Phishing** | Domain Registrar, Hosting Provider | security.txt, abuse@domain |

#### Step 2: Prepare Abuse Report

**Essential Information:**
- **URL/IP/Domain:** Exact resource reported
- **Threat Type:** Phishing, malware, botnets, spam, etc.
- **Evidence:** Screenshots, VirusTotal link, URLhaus reference
- **Timeline:** When discovered, if still active
- **Impact:** Affected systems/users (if relevant)

**Template (modify as needed):**
```
Subject: Abuse Report - [Phishing/Malware] Infrastructure

Hello,

We report malicious infrastructure hosted on your network:

IP Address: 192.0.2.100
Domain: phishing.example.com
Threat: Phishing campaign targeting financial institution users
Evidence: https://www.virustotal.com/gui/url/[URL]/detection

Timeline: First observed [DATE]; still active as of [DATE].

Please confirm receipt and advise on takedown timeline.

[Your name/organization]
```

#### Step 3: Send Report

- **Registrar Abuse:** abuse@[registrar domain] or security contact from WHOIS
- **ISP/Hosting:** abuse@[ISP domain]; use ARIN RDAP for abuse contact
- **Law Enforcement (severe cases):** IC3.gov (FBI), regional cybercrime units

#### Step 4: Follow Up

- **Wait 48–72 hours** for initial response
- **Escalate if no response:** Contact registrar's escalation team or law enforcement
- **Document all communication** for audit trail

### Community Reporting (Public Participation)

- **AbuseIPDB:** Submit IP abuse reports (categorized)
- **PhishTank:** Submit phishing URLs (verifications required)
- **URLhaus:** Submit malware distribution URLs
- **abuse.ch:** Submit malware samples, C2 infrastructure

**Caution:** Community reports require verification; cross-check with primary sources.

---

## Tools Summary Table

| Tool | Data Type | Cost | Best Use | Confidence |
|------|-----------|------|----------|-----------|
| **VirusTotal** | Files, URLs, domains, IPs | Freemium | Rapid multi-engine assessment | High (consensus) |
| **URLhaus** | Malware URLs, hosting | Free | Malware C2, distribution tracking | Medium-High |
| **PhishTank** | Phishing URLs | Free | Phishing campaign tracking | Medium-High |
| **AbuseIPDB** | IP abuse reports | Freemium | IP reputation, brute-force sources | Medium |
| **abuse.ch (suite)** | Malware, C2, SSL abuse | Free | Researcher-curated threat intel | High |
| **Spamhaus** | Botnet/malware/phishing | Free | Blocklist, DNS filtering | High |
| **SURBL** | Email reputation | Free | Email gateway filtering | Medium-High |

---

## Integration with SOC Workflows

**Alert Triage Pattern:**

1. Extract IP/domain/URL from alert
2. Query VirusTotal (5 sec)
3. If flagged: Check community score + relationships (2 min)
4. Query AbuseIPDB (1 min)
5. Decision: True Positive (escalate) vs. False Positive (suppress)

**Incident Response Pattern:**

1. Discover malicious IP (192.0.2.100)
2. Query Spamhaus DROP → Confirmed botnet C&C
3. Query ARIN RDAP → Identify hosting provider
4. Report to hosting provider abuse contact
5. Request forensic retention (evidence handling)
6. Follow up after 72 hours

---

## Next Steps

- **[OSINT Foundations →](../foundations/README.md)** — Core methodology and verification.
- **[DNS Intelligence →](../dns/README.md)** — Investigating network infrastructure.
