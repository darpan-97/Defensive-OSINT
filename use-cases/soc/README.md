# Use-Case: SOC Alert Triage & Enrichment

**Audience:** Security Operations Center analysts, 1st/2nd-tier response teams  
**Goal:** Rapidly assess alert legitimacy, enrich with context, reduce false positives, inform escalation decisions  
**Typical Timeline:** 2–5 minutes per alert (real-time operations); 10–15 minutes for complex pivots

---

## Overview

SOC alert triage is the frontline of incident response. This playbook enables analysts to:

1. **Classify alerts** by threat level and type
2. **Enrich with context** via OSINT (reputation, infrastructure)
3. **Detect false positives** early (shared hosting, legitimate tools, misconfiguration)
4. **Escalate confidently** with documented reasoning
5. **Tune detections** based on findings to reduce future noise

### Key Principles

- **Speed Matters:** Rapid triage prevents alert fatigue and ensures critical alerts receive timely attention
- **Verify Assumptions:** Reputation signals are not proof; always look for corroborating evidence
- **Document Everything:** Case notes must be reproducible and audit-trail–friendly
- **Escalate Uncertainty:** If in doubt, escalate rather than suppress

---

## Alert Types & Response Templates

### Type 1: Suspicious Domain/URL (Phishing, Malware Distribution)

**Typical Alert Source:** Email gateway, proxy/firewall, endpoint detection

**Quick Triage Workflow (5 minutes):**

| Step | Action | Tool | Output |
|------|--------|------|--------|
| 1 | Extract domain/URL | Alert logs | `phishing.example.com` |
| 2 | Query reputation | VirusTotal | Flag count, community score, oldest report date |
| 3 | Check registrant | WHOIS/SecurityTrails | Domain registrant, registrar, registrant correlation |
| 4 | Assess phishing signals | PhishTank, URLhaus | "Phishing confirmed" or "Unknown" |
| 5 | Grade confidence | Decision tree (see below) | True Positive (TP) / False Positive (FP) |

**Decision Tree:**

```
Query VirusTotal:
  ├─ Flagged by 5+ AV engines? → TP (High confidence)
  │  └─ Action: Escalate to incident response
  ├─ Flagged by 1–4 engines + phishing report? → TP (Medium)
  │  └─ Action: Escalate; monitor for spread
  ├─ No flags + domain age < 7 days + typosquatting pattern? → TP (Medium)
  │  └─ Action: Notify brand protection; prepare takedown
  ├─ Flagged only as "clean" by multiple engines + legitimate domain? → FP
  │  └─ Action: Suppress; update allowlist
  └─ Ambiguous? → Escalate to L2/L3 for manual investigation
```

**Case Notes Template:**

```
**Alert ID:** [ALERT_ID]
**Date/Time:** [TIMESTAMP]
**IoC:** [DOMAIN/URL]
**Alert Source:** [GATEWAY/PROXY/EDR]
**Summary:** [1 sentence describing alert]

**Triage Findings:**
- VirusTotal: [Flag count]/70; community score: [X]
- URLhaus: [Listed as phishing/malware/unknown]
- Domain Age: [X days]
- Registrant: [Info or "Privacy Protected"]
- Targeted Recipient(s): [Email addresses affected]

**Confidence Assessment:** [TP/FP]
**Reasoning:** [Why you classified it as TP/FP]
**Next Steps:** [Escalate to IR / Suppress / Monitor]
**Analyst:** [Your name]
```

### Type 2: Suspicious IP (Brute Force, Botnet C&C, Proxy)

**Typical Alert Source:** IDS/IPS, firewall, EDR (command & control)

**Quick Triage Workflow (3–5 minutes):**

| Step | Action | Tool | Output |
|------|--------|------|--------|
| 1 | Extract IP | Alert logs | `192.0.2.100` |
| 2 | Query AbuseIPDB | AbuseIPDB | Abuse score, report count, categories |
| 3 | Check hosting | ARIN RDAP | ASN, provider, datacenter type |
| 4 | Query reputation | VirusTotal, Spamhaus | Known malware C&C, botnet |
| 5 | Assess false positives | Check for VPN/proxy/CDN | True IP vs. masked origin |
| 6 | Grade confidence | Decision tree | TP / FP / Escalate |

**Decision Tree:**

```
AbuseIPDB Abuse Score:
  ├─ >90% + Spamhaus DROP match → TP (High); known botnet
  │  └─ Action: Block at firewall; escalate to IR
  ├─ 50–90% + SSH Brute Force category → TP (Medium); external threat
  │  └─ Action: Monitor; block if internal services exposed
  ├─ 25–50% + Multiple reports (10+) → TP (Medium); suspicious
  │  └─ Action: Escalate to IR for correlation
  ├─ <25% + residential ISP ASN → FP (High); likely false positive
  │  └─ Action: Suppress; check if user behavior is expected
  ├─ Datacenter IP + no reports → Ambiguous; context needed
  │  └─ Action: Check source logs (source IP, source port, protocol)
  └─ Check if IP is: VPN, Proxy, CDN (CloudFlare, Akamai)
     └─ Yes: FP; legitimate service; refine detection rule
```

**IP Context Rapid Check:**

```bash
# Query ARIN for hosting provider
whois -h whois.arin.net 192.0.2.100

# Check Shodan/Censys for open ports (web interface)
# Check AbuseIPDB for report categories (SSH, proxy, DDoS)
```

**Case Notes Template:**

```
**Alert ID:** [ALERT_ID]
**IP Address:** 192.0.2.100
**Alert Type:** [Brute Force / C&C / Proxy Traffic]
**Source/Destination:** [Internal host → External IP]

**Triage Findings:**
- AbuseIPDB Score: [X]%; Reports: [N]
- Top Categories: [SSH, Proxy, etc.]
- ASN: [AS12345]; Provider: [Hosting Company]
- Datacenter: Yes/No; ASN Type: [Residential/Hosting/CDN]
- Spamhaus: [Blacklisted/Clean]
- VirusTotal: [Flagged as C&C/Clean]

**Assessment:** [TP/FP/Uncertain]
**Reasoning:** [Key findings]
**Action:** [Block / Monitor / Suppress]
**Analyst:** [Your name]
```

### Type 3: Suspicious Process/Hash (Malware, Exploit Kit)

**Typical Alert Source:** EDR, antivirus, behavioral detection

**Quick Triage Workflow (3–7 minutes):**

| Step | Action | Tool | Output |
|------|--------|------|--------|
| 1 | Extract file hash | Alert logs | SHA256 hash |
| 2 | Query VirusTotal | VirusTotal | AV flagging count, first-seen date |
| 3 | Check relationships | VT Relationships | Related domains, IPs, campaigns |
| 4 | Research malware family | MalwareBazaar, threat reports | Known malware, TTPs |
| 5 | Assess payload | Sandbox report (if available) | Network IOCs, registry changes |
| 6 | Grade confidence | Flagging threshold | TP / FP / Uncertain |

**Decision Tree:**

```
VirusTotal Flag Count:
  ├─ 10+ AV flags → TP (High); known malware
  │  └─ Action: Isolate host immediately; escalate to IR
  ├─ 5–9 flags + first-seen < 7 days → TP (Medium); emerging threat
  │  └─ Action: Isolate; check for lateral movement
  ├─ 2–4 flags + commercial detection tools (ESET, Kaspersky, etc.) → TP (Medium)
  │  └─ Action: Isolate; monitor
  ├─ 1 flag + obscure vendor + hash age > 1 year → FP (Medium)
  │  └─ Action: Check for legitimate software (installers, etc.)
  ├─ 0 flags + hash < 24h old → Uncertain; may be new malware
  │  └─ Action: Check process behavior (network, registry, files)
  │    └─ Malicious behavior observed: Escalate to IR
  │    └─ Normal behavior: Suppress; may be false positive
  └─ Known good (OS files, legitimate software) → FP; suppress
```

**Case Notes Template:**

```
**Alert ID:** [ALERT_ID]
**File Hash (SHA256):** [HASH]
**File Name:** [FILENAME]
**Execution Context:** [Process path, parent process, command-line]

**Triage Findings:**
- VirusTotal Flags: [X]/70
- Malware Family: [Name] or [Unknown]
- First Submission: [Date]
- Sandbox Behavior: [Malicious/Suspicious/Clean]
- Network IOCs: [IPs, domains contacted]
- Related Campaign: [If known from threat reports]

**Assessment:** [TP/FP/Uncertain]
**Reasoning:** [Key evidence]
**Action:** [Isolate / Block / Suppress / Monitor]
**Escalation Details:** [Affected hosts, lateral movement indicators]
**Analyst:** [Your name]
```

---

## Enrichment Data Sources (Quick Reference)

| IoC Type | Primary Source | Secondary Source | Time Budget |
|----------|----------------|------------------|------------|
| **Domain** | VirusTotal + WHOIS | SecurityTrails, PhishTank | 3–5 min |
| **URL** | VirusTotal + URLhaus | PhishTank, proxy logs | 2–3 min |
| **IP** | AbuseIPDB + ARIN RDAP | VirusTotal, Spamhaus, Shodan | 3–5 min |
| **File Hash** | VirusTotal + MalwareBazaar | Threat reports, YARA rules | 3–7 min |
| **Email** | WHOIS (sender domain) + SPF/DKIM | PhishTank, phishing-specific feeds | 3–5 min |

---

## Alert Correlation & Pattern Recognition

### Pattern 1: Phishing Campaign Spread

**Scenario:** Multiple alerts for similar phishing domains (typosquatting, lookalikes)

**Investigation Steps:**

1. Extract all domain names from alerts
2. Query SecurityTrails for each → Get nameserver, registrant, hosting IP
3. Identify commonalities:
   - Same registrant? → Same attacker
   - Same registrar? → Same registrar account (possible)
   - Same nameserver? → Strong link
   - Same IP block? → Same hosting provider (possible but weaker signal)
4. Create domain timeline → Registrant correlation report
5. **Action:** Full campaign takedown package to registrar/hoster

### Pattern 2: C&C Communication Spike

**Scenario:** Multiple internal hosts attempting connections to same external IP

**Investigation Steps:**

1. Extract IP from alerts; query Spamhaus, VirusTotal
2. If confirmed C&C:
   - Query ASN → Identify bulletproof hosting provider
   - Check passive DNS history → Find related domains
   - Query URLhaus → Known malware distribution URLs
3. Check for common malware family → Search EDR for related hashes
4. **Action:** Network segmentation, host isolation, forensic preservation

---

## False Positive Suppression & Tuning

### Common False Positive Sources

| False Positive | Root Cause | Mitigation |
|----------------|-----------|-----------|
| **Corporate VPN IP** | AbuseIPDB reports from shared VPN | Whitelist corporate VPN ranges |
| **CDN-hosted legitimate site** | Single malware report on old version | Verify domain ownership; suppress if legitimate |
| **Build server** | Process execution alerts on deployment systems | Whitelist build tools; adjust detection thresholds |
| **Shared cloud provider IP** | Thousands of sites on same IP | Check domain reputation separately; don't block IP blindly |
| **SSL certificate issue alerts** | Misconfiguration or expired cert on legitimate domain | Verify domain ownership; coordinate with app teams |

### Feedback Loop for Detection Engineers

**Weekly SOC Triage Review:**

- Collect suppressed alerts
- Analyze patterns (which alerts are false positives?)
- Work with detection engineering to refine signatures
- Update whitelists/blacklists
- Document changes for reproducibility

---

## SOC Playbook Checklist

### Before Starting Shift
- [ ] Verify access to VirusTotal, AbuseIPDB, WHOIS tools
- [ ] Check for updated threat feeds or known campaigns
- [ ] Review on-call escalation contacts

### For Each Alert
- [ ] Extract IoCs (domain, IP, hash, email)
- [ ] Query primary reputation tool (VirusTotal/AbuseIPDB)
- [ ] Document findings in case management system
- [ ] Classify as TP/FP with reasoning
- [ ] Escalate or suppress with documented justification
- [ ] Update whitelist/blacklist if needed

### End of Shift
- [ ] Summarize high-impact alerts for handoff
- [ ] Highlight patterns (campaigns, new threats)
- [ ] Submit feedback to detection engineering team

---

## Tool Integration & Automation

### SOAR/Orchestration Workflow

Most modern SOC platforms (e.g., Splunk SOAR, Demisto) can automate triage via playbooks:

```
Trigger: Alert Generated
├─ Extract IoC
├─ Query VirusTotal API
├─ Query AbuseIPDB API
├─ Enrich with passive DNS (SecurityTrails)
├─ Check GDPR compliance (WHOIS privacy)
├─ Generate confidence score
└─ Route to analyst with auto-populated context
```

**Recommended Integrations:**
- VirusTotal API for file/URL/domain/IP queries
- AbuseIPDB API for IP reputation
- SecurityTrails API for passive DNS enrichment
- CIRCL Passive DNS for non-commercial use

---

## Key Resources & References

- [VirusTotal Guide](../../core/reputation-abuse-reporting/#virustotal)
- [AbuseIPDB Reference](../../core/reputation-abuse-reporting/#abuseipdb)
- [WHOIS & RDAP Lookup](../../core/domains-whois-rdap/)
- [Passive DNS Queries](../../core/dns/)
- [OSINT Foundations & Confidence Scoring](../../core/foundations/)

---

## Next Steps

- **Escalate to IR?** See [DFIR Investigation](../../use-cases/dfir/)
- **Brand protection?** See [Phishing Protection](../../use-cases/phishing-brand/)
- **Threat hunting?** See [/use-cases/threat-hunting/](use-cases/threat-hunting/)
- **Build detection rules?** See [/core/automation-pipelines/](core/automation-pipelines/)
