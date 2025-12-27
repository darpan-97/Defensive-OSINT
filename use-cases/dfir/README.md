# Use-Case: Digital Forensics & Incident Response (DFIR)

**Audience:** Incident response teams, digital forensics analysts, threat intelligence analysts  
**Goal:** Systematically map threat infrastructure, establish timeline, correlate with known threat actors, preserve evidence  
**Typical Timeline:** 1–2 hours for scoping; ongoing for full investigation

---

## Overview

DFIR extends beyond alert triage into comprehensive infrastructure attribution and evidence handling. This playbook enables teams to:

1. **Scope incidents** by identifying all affected systems and infrastructure
2. **Map threat infrastructure** across domains, IPs, ASNs, and hosting providers
3. **Establish timeline** via historical DNS, WHOIS, and certificate data
4. **Correlate with threat actors** using infrastructure patterns and TTPs
5. **Document evidence** for legal defensibility and operational handoff

### Key Differences from SOC Triage

| Aspect | SOC Triage | DFIR Investigation |
|--------|-----------|-----------------|
| **Timeframe** | 2–5 minutes | Hours to days |
| **Depth** | Surface-level reputation | Deep infrastructure mapping |
| **Corroboration** | Single source per IoC | Multiple independent sources |
| **Output** | TP/FP decision | Detailed incident report, evidence package |
| **Evidence Standard** | Audit-friendly | Legal-defensible (chain of custody) |

---

## Investigation Phases

### Phase 1: Scoping & Evidence Preservation (First 1–2 hours)

**Objective:** Identify scope of compromise and preserve evidence before it disappears

#### Step 1: Collect Observables

From available sources:
- Alert telemetry (EDR, IDS/IPS, firewall, endpoint logs)
- Email headers (phishing emails)
- Network captures (PCAP files)
- Memory dumps (if possible)
- File system artifacts (log files, temporary files)

**Data to Preserve:**
- Original evidence (never modify; use copies)
- Metadata (timestamps, access times)
- Full context (not just IoCs; include logs, screenshots)

#### Step 2: Extract Initial IoCs

| IoC Type | Source | Preservation Method |
|----------|--------|-------------------|
| **Domain/IP** | Alert, email headers, network logs | Screenshot + WHOIS export + passive DNS archive |
| **File Hash** | EDR, antivirus | File preservation in evidence storage |
| **Email** | Gateway logs | Full email export (headers + body + attachments) |
| **URL** | Email, web logs | Screenshot + URL decode + VirusTotal snapshot |

**Template for IoC Collection:**

```
**Investigation ID:** [INC-2025-00123]
**Timeline Start:** [YYYY-MM-DD HH:MM UTC]
**Initial IoCs Extracted:**

| IoC | Type | Source | Timestamp | Notes |
|-----|------|--------|-----------|-------|
| phishing.example.com | Domain | Email alert | 2025-12-27 10:45 UTC | Phishing landing page |
| 192.0.2.100 | IP | Web proxy log | 2025-12-27 10:50 UTC | Malware C&C |
| [HASH] | File SHA256 | EDR alert | 2025-12-27 11:00 UTC | Trojan downloader |
```

#### Step 3: Establish Communication & Escalation

- Assign incident commander
- Establish status meeting cadence (e.g., every 2 hours)
- Identify stakeholders (IT, security, legal, management)
- Determine containment scope (if immediate action needed)

### Phase 2: Infrastructure Mapping (2–8 hours)

**Objective:** Build a complete picture of threat infrastructure to identify all related indicators

#### Step 1: Domain-Centric Investigation

**From suspicious domain `phishing.example.com`:**

1. **WHOIS Lookup:**
   ```
   Domain: phishing.example.com
   Registrar: BadRegistrar Inc.
   Registrant: John Doe [privacy protected]
   Nameservers: ns1.bulletproof.com, ns2.bulletproof.com
   Created: 2025-12-20
   Last Updated: 2025-12-26
   ```

2. **Identify Registrant Correlation:**
   - Query WHOIS database for all domains with registrant "John Doe"
   - Or find all domains using nameservers ns1.bulletproof.com, ns2.bulletproof.com
   - **Result:** Discover 15 similar phishing domains (campaign scope)

3. **Passive DNS History:**
   - Query SecurityTrails for historical DNS records
   - Timeline: Domain resolved to 192.0.2.100 (Dec 20–26), then 192.0.2.101 (Dec 26–present)
   - **Finding:** Infrastructure rotation; suggest compromised/rotating hosting

4. **Certificate Transparency Search:**
   - Search for all certs issued for `*.phishing.example.com`
   - Find subdomains not in DNS: `admin.phishing.example.com`, `mail.phishing.example.com`
   - **Expansion:** Discover additional infrastructure for deeper pivoting

#### Step 2: IP-Centric Investigation

**From suspicious IP `192.0.2.100`:**

1. **ARIN RDAP Lookup:**
   ```
   IP: 192.0.2.100
   CIDR: 192.0.2.0/24
   Provider: Hosting Corp (AS12345)
   Type: Datacenter/Bulletproof Hosting
   Abuse Contact: abuse@hostingcorp.com
   ```

2. **Passive DNS Reverse Lookup:**
   - Find all domains that resolved to 192.0.2.100
   - **Result:** 40+ domains (phishing, malware distribution, C2)
   - Time-correlate: Domains created within 2–3 days of each other → Single attacker, single campaign

3. **BGP/ASN Analysis:**
   - AS12345 is known bulletproof hosting (check AbuseIPDB, threat reports)
   - Query BGPView for AS12345 peers and upstream providers
   - **Finding:** Provider tolerates abuse; note for takedown strategy

4. **Reputation Checks:**
   - Spamhaus: 192.0.2.100 listed as botnet C&C
   - AbuseIPDB: 200+ abuse reports, all from security researchers
   - **Confirmation:** Known malicious infrastructure

#### Step 3: Certificate & Service Fingerprinting

**Query Censys/Shodan for IP `192.0.2.100`:**
- Open ports: 80 (HTTP), 443 (HTTPS), 25 (SMTP)
- TLS certificate: Self-signed, common name "server.example.com"
- HTTP banner: Custom phishing server (identifies attacker toolkit)

**Finding:** Distinctive certificate allows attribution to other IPs using same cert

#### Step 4: Infrastructure Clustering

**Synthesize findings into infrastructure map:**

```
ATTACKER CAMPAIGN: "Phishing Wave Alpha"
├─ Registrant: John Doe (privacy-protected WHOIS)
├─ Registrar: BadRegistrar Inc.
├─ Nameservers: ns1/ns2.bulletproof.com (AS12345)
├─ Hosting Provider: Hosting Corp (AS12345)
├─ Timeline: Created Dec 20, 2025; rotating IPs every 3–5 days
├─ Infrastructure:
│  ├─ Domains (15 total):
│  │  ├─ phishing.example.com (Dec 20–26)
│  │  ├─ phising.example.com (Dec 20–27)
│  │  ├─ phishng.example.com (Dec 21–27)
│  │  └─ [12 more similar typosquats]
│  ├─ IPs (3 observed):
│  │  ├─ 192.0.2.100 (Dec 20–23)
│  │  ├─ 192.0.2.101 (Dec 23–26)
│  │  └─ 192.0.2.102 (Dec 26–present)
│  └─ Services:
│     ├─ Phishing landing page (HTTP 200)
│     ├─ SMTP mail relay
│     └─ Custom malware payload delivery
└─ Threat Correlation:
   ├─ TTP: Typosquatting (domain)
   ├─ TTP: Bulletproof hosting (evasion)
   ├─ TTP: Fast IP rotation (evasion)
   └─ Similar to: Phishing group "BadActors Inc." (unconfirmed)
```

### Phase 3: Threat Actor Correlation (2–4 hours)

**Objective:** Determine if infrastructure links to known threat actors

#### Methodology (Unit 42 Attribution Model)

1. **Gather all infrastructure attributes:**
   - Registrant patterns (names, emails, phone numbers)
   - Registrar preferences
   - Nameserver choices
   - Hosting provider preferences
   - TTP alignment (typosquatting, fast rotation, etc.)
   - Malware families used
   - Victimology (targeted industries/geographies)

2. **Compare to known threat actor profiles:**
   - Search threat databases (Mandiant, Dragos, etc.)
   - Look for overlapping infrastructure
   - Compare TTPs via MITRE ATT&CK
   - Cross-reference with open-source research (Bellingcat, Unit 42)

3. **Assign confidence level:**

| Confidence | Criteria | Example |
|-----------|----------|---------|
| **High** | Multiple independent infrastructure overlaps + TTP alignment + public attribution | Same registrant, same registrar, same nameserver + matches known group's victimology |
| **Medium** | 1–2 overlaps + some TTP alignment | Same registrar + same toolkit, but different registrant |
| **Low** | Surface-level similarity; alternative explanations plausible | Similar TTP, but widely available tools; could be multiple actors |

**Example Finding:**
```
Confidence: MEDIUM
Suspected Attribution: PhishingGroup "BadActors Inc."

Evidence:
├─ Infrastructure Overlap:
│  ├─ Same registrar (BadRegistrar Inc.) as 3 prior BadActors campaigns
│  ├─ Nameserver reuse (ns1.bulletproof.com) with 2 prior campaigns
│  └─ IP range overlap (192.0.2.0/24) with 1 prior campaign
├─ TTP Alignment:
│  ├─ Domain typosquatting (consistent with BadActors methodology)
│  ├─ Fast IP rotation (signature evasion technique)
│  └─ Targeted financial institutions (consistent victimology)
├─ Counter-Evidence:
│  └─ Registrant name differs from prior campaigns (could be new identity)
└─ Next Steps:
   ├─ Monitor for additional infrastructure using same registrar
   ├─ Cross-reference malware family with BadActors known tools
   └─ Coordinate with law enforcement if medium+ confidence achieved
```

### Phase 4: Incident Report & Recommendations

**Report Structure:**

1. **Executive Summary**
   - What happened?
   - How many systems affected?
   - Estimated business impact
   - Key recommendations

2. **Incident Details**
   - Timeline of events
   - Affected assets (systems, users, data)
   - Indicators of Compromise (IoC list)
   - Threat actor attribution (if applicable)

3. **Technical Analysis**
   - Infrastructure map (diagram)
   - Malware analysis (if relevant)
   - Evidence chain of custody

4. **Recommendations**
   - Immediate (containment, notification)
   - Short-term (patching, user communication)
   - Long-term (detection rules, architectural changes)

5. **Appendices**
   - Full IoC list (STIX/MISP format for sharing)
   - Passive DNS records (exported)
   - WHOIS snapshots
   - Evidence screenshots

---

## Tools & Data Sources Quick Reference

| Task | Primary Tool | Secondary Tool |
|------|-------------|-----------------|
| **WHOIS lookup** | ARIN RDAP / whois CLI | SecurityTrails |
| **Passive DNS history** | SecurityTrails | CIRCL Passive DNS |
| **Certificate search** | SSLMate / Censys | crt.sh |
| **ASN/BGP analysis** | BGPView | RIPEstat |
| **IP reputation** | AbuseIPDB + Spamhaus | VirusTotal |
| **Domain enumeration** | SecurityTrails reverse DNS | DNSDumpster |
| **Threat attribution** | Unit 42 framework + threat reports | MITRE ATT&CK |
| **Evidence storage** | Local DFIR workstation + encrypted cloud | Splunk, ELK |

---

## Evidence Handling Best Practices

### Chain of Custody

For any evidence that may be used in legal proceedings:

1. **Document source:** Where did the data come from?
2. **Record access:** Who accessed it, when, and why?
3. **Preserve hash:** Calculate MD5/SHA256 of original files
4. **Minimize changes:** Use copies; never modify originals
5. **Maintain timeline:** Document all metadata (timestamps)

**Template:**
```
Evidence ID: [EVI-INC-123-001]
Description: WHOIS export for phishing.example.com
Source: whois CLI query to ARIN, 2025-12-27 12:30 UTC
Collected By: [Name, Title]
Preserved As: [Filename, storage location, hash]
Access Log:
  - 2025-12-27 12:30 UTC: Collected by [Name]
  - 2025-12-27 14:00 UTC: Reviewed by incident commander
```

---

## Next Steps

- **Phishing investigation?** See [Phishing Protection](../../use-cases/phishing-brand/)
- **Threat hunting for related activity?** See [/use-cases/threat-hunting/](use-cases/threat-hunting/)
- **Build detection rules?** See [/core/automation-pipelines/](core/automation-pipelines/)
- **Structure findings for sharing?** See [/core/threat-intel/](core/threat-intel/) for STIX/MISP format
