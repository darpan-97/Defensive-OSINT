# Core: OSINT Foundations & Methodology

This section establishes the **principles, verification techniques, and legal/ethical framework** underlying all OSINT activities in this repository.

---

## OSINT Methodology Framework

### The OSINT Analytical Process

OSINT is not about tools—it's about **structured analysis of publicly available information**. The methodology follows this cycle:

1. **Requirements Definition** — What question are you answering? (e.g., "Who owns this domain?" vs. "What infrastructure links these two threat actors?")
2. **Information Gathering** — Passive collection from official sources (DNS, WHOIS, certificate logs, public registries)
3. **Verification & Corroboration** — Cross-check findings across multiple independent sources to confirm accuracy
4. **Analysis & Pivoting** — Identify relationships between data points (domain → IP → ASN → organization)
5. **Confidence Assessment** — Assign credibility scores based on evidence quality and source reliability
6. **Documentation & Reporting** — Write findings in a manner that allows reproducibility and legal defensibility

### Key Principles

- **Single Source of Truth Caveats:** No single database is authoritative for all infrastructure data. Always corroborate.
- **Temporal Awareness:** DNS records, IP ownership, and certificate histories change over time. Document timestamps.
- **Attribution vs. Association:** Observing shared infrastructure does not prove common ownership or control. Use confidence frameworks for assessment.
- **Legal & Jurisdictional Boundaries:** What is legal in one jurisdiction may not be in another. Understand your environment before acting.

---

## Verification & Corroboration Techniques

### Evidence Quality Hierarchy

| Tier | Example | Reliability | Notes |
|------|---------|-------------|-------|
| **Primary** | IANA registry, RIR WHOIS, CT logs | High | Official source; auditable; append-only |
| **Secondary** | Passive DNS aggregators, certificate search APIs | Medium-High | Dependent on source credibility + data freshness |
| **Tertiary** | Security vendor reports, forum discussions | Medium | Useful for context; always verify with primary sources |
| **Community Signal** | Reddit discussions, Twitter threads | Low | Subject to false positives; validate before acting |

### Four-Pillar Corroboration

For any claim (e.g., "This IP hosts phishing infrastructure"), validate via:

1. **Passive Artifacts** — DNS, WHOIS, certificate records
2. **Reputation Signals** — Blocklists, abuse reports, threat intel feeds
3. **Behavioral Patterns** — TTPs (tactics, techniques, procedures) alignment with known threat actors
4. **Temporal Consistency** — Timeline correlation across multiple data points

If only one pillar supports your finding, mark confidence as **Low**.

---

## Evidence Handling & Documentation

### Chain of Custody for Digital Investigations

When documenting OSINT findings for legal/incident response contexts:

1. **Source & Timestamp** — Record the exact tool/URL and access time (include timezone)
2. **Data Capture** — Screenshot, export, or archive the original data (avoid manual transcription)
3. **Preservation** — Store evidence in a tamper-evident manner (hash files, use immutable logs)
4. **Analysis Notes** — Document your reasoning, assumptions, and any transformations applied
5. **Limitations** — Explicitly note caveats (e.g., "DNS record reflects state at 2025-12-27T22:45:00 UTC; current state may differ")

### Template for Evidence Documentation

```
**Source:** [Tool name + URL]
**Access Time:** [YYYY-MM-DD HH:MM:SS ± Timezone]
**Data Captured:** [Brief description of what was retrieved]
**Finding:** [Your interpretation of the data]
**Confidence:** [High/Medium/Low + reasoning]
**Limitations:** [What this data does NOT tell us]
**Corroboration:** [Which other sources confirm this finding?]
**Analyst Notes:** [Judgment calls, ambiguities, next steps]
```

---

## Operational Safety & Legal Guidance

### Passive vs. Active Reconnaissance

| Activity | Type | Legal Status | Safety |
|----------|------|--------------|--------|
| Query public WHOIS | Passive | Legal in most jurisdictions | Safe; use official APIs |
| Search certificate logs | Passive | Legal | Safe; designed for public access |
| Query Shodan/Censys | Passive | *Varies by jurisdiction* | **Caution:** May trigger monitoring alerts on some networks |
| DNS queries to public resolvers | Passive | Legal | Safe; commonplace |
| Port scanning without authorization | Active | **Illegal** in many jurisdictions | **Never do this** |
| Accessing non-public data | Active | **Illegal** | **Never do this** |

### Jurisdictional Considerations

- **EU:** GDPR restricts passive collection of personally identifiable information from WHOIS. Use RDAP where available.
- **USA:** CFAA (Computer Fraud and Abuse Act) prohibits unauthorized access; passive OSINT is generally legal.
- **Other regions:** Check local regulations before deploying tools or exporting findings.

### Responsible Disclosure

If your OSINT research uncovers a security issue (e.g., exposed credentials, vulnerable infrastructure):

1. **Verify** the issue is real (false positives are common)
2. **Identify** the responsible party (organization owner, hosting provider)
3. **Report privately** via abuse contacts, security.txt, or vendor security teams
4. **Wait** for patch/response (typically 90 days) before public disclosure
5. **Document** your disclosure timeline for credibility

See [Reputation & Abuse](../reputation-abuse-reporting/README.md) for abuse contact workflows.

---

## Red Flags & False Positives

### Common OSINT Mistakes

- **Shared Hosting Attribution Errors** — Hundreds of legitimate sites share hosting IP addresses. A malicious site on the same IP does NOT implicate other sites.
- **GDPR Privacy Proxies** — WHOIS privacy masks legitimate businesses; absence of registrant data is not evidence of malice.
- **CDN/Reverse Proxy Confusion** — An IP blocking at Censys may be a CDN node, not the actual target server.
- **Temporal Lag** — Passive DNS databases may lag 24–48 hours; recent registrations won't appear immediately.
- **DNS Rebinding** — Domains may resolve to different IPs from different geographic locations.

### How to Avoid False Positives

1. **Always check current state** — Tools aggregate historical data; re-query official sources
2. **Understand data age** — Ask "When was this record last updated?"
3. **Cross-check with reputation platforms** — A legitimate domain flagged by one vendor may not be flagged by others
4. **Review ASN/hosting context** — Shared hosting, CDNs, and cloud providers host both good and bad sites
5. **Apply Occam's Razor** — The simplest explanation is usually correct; extraordinary claims require extraordinary evidence

---

## Attribution Confidence Framework

Adapted from Unit 42 Attribution Model and Admiralty System:

### Admiralty Credibility Ratings

| Score | Rating | Meaning |
|-------|--------|---------|
| **1** | Confirmed | Multiple independent sources corroborate; auditable evidence |
| **2** | Probably True | Majority of sources agree; minor gaps remain |
| **3** | Possibly True | Plausible; some supporting evidence; significant gaps |
| **4** | Doubtfully True | Weak evidence; many alternative explanations; high uncertainty |
| **5** | Improbable | Contradicted by multiple sources; unlikely |
| **6** | Cannot Judge | Insufficient information; defer assessment |

### Confidence Assessment Template

**Claim:** [Specific assertion: e.g., "Domain X is associated with threat actor Y"]

**Evidence:**
- Evidence A: [Source, credibility score, reasoning]
- Evidence B: [Source, credibility score, reasoning]
- Evidence C: [Source, credibility score, reasoning]

**Conflicting Evidence:**
- Counter-argument A: [Why this might be wrong]

**Overall Confidence:** [1–6 + narrative justification]

**Next Steps:** [What additional evidence would increase confidence?]

---

## OSINT Tools Ethics & Terms of Service

### Golden Rules

1. **Respect ToS** — Each tool has terms of service. Understand rate limits, automated access policies, and commercial use restrictions.
2. **No Scraping Without Permission** — Many OSINT platforms prohibit automated scraping; use official APIs where available.
3. **Attribute Data** — If publishing findings, cite your sources and respect data ownership.
4. **Disclose Limitations** — Be explicit about what your data does NOT show.

### Example ToS Considerations

- **Shodan / Censys:** Internet-wide scanning data; permitted for defensive use; check ToS for commercial licensing
- **VirusTotal:** Aggregated threat intelligence; free tier for personal use; enterprise licensing for commercial/large-scale use
- **WHOIS / RDAP:** Registry data; personal data (GDPR) restrictions apply
- **CIRCL Passive DNS:** Community resource; free access; attribute data source

---

## Self-Assessment: OSINT Practitioner Maturity

### Novice
- [ ] Understand the difference between passive and active reconnaissance
- [ ] Know how to query WHOIS, DNS, and certificate logs
- [ ] Can identify basic false positives (shared hosting, CDNs)
- [ ] Document findings in reproducible manner

### Intermediate
- [ ] Correlate findings across multiple data sources
- [ ] Apply confidence frameworks to findings
- [ ] Understand geopolitical/jurisdictional context
- [ ] Identify infrastructure relationships (ASN, registrar pivots)

### Advanced
- [ ] Attribute activity to threat actors with high confidence
- [ ] Detect and bypass evasion techniques (privacy proxies, bulletproof hosting)
- [ ] Build automated enrichment pipelines
- [ ] Contribute to threat intelligence feeds (STIX/MISP format)

---

## Resources for Further Learning

### Academic & Research

- **Unit 42 Attribution Framework** → [Palo Alto Networks Unit 42](https://unit42.paloaltonetworks.com/)
- **Admiralty System** → [NATO Intelligence Standards](https://en.wikipedia.org/wiki/Admiralty_Code)
- **Diamond Model** → [Dragos / Mandiant documentation](https://www.diamondmodel.org/)
- **MITRE ATT&CK** → [Tactics, Techniques & Procedures](https://attack.mitre.org/)

### Practitioner Communities

- **Bellingcat Investigative Journalism** → [Bellingcat.com](https://www.bellingcat.com/) (methodology-focused case studies)
- **SANS Internet Storm Center** → [DShield.org](https://isc.sans.edu/) (threat trends, best practices)
- **r/OSINT & r/threatintel** → Reddit communities with peer review

---

## Next Steps

- **[DNS Intelligence →](../dns/README.md)** — Investigating network infrastructure.
- **[Reputation & Abuse →](../reputation-abuse-reporting/README.md)** — Investigating malicious infrastructure.
