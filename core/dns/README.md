# Core: DNS Intelligence

**Primary Data Type:** DNS query/response records, nameserver configurations, DNS record history  
**Primary Pivots:** Domain → IP, IP → Domain, Nameserver → Domains  
**Common Use:** Passive infrastructure mapping, malware C2 detection, domain registration patterns

---

## Overview

DNS is the **foundational layer** of internet infrastructure. Passive DNS data reveals historical and current domain-to-IP mappings, enabling defenders to:

- Identify all domains hosted on a malicious IP
- Track infrastructure changes over time
- Detect fast-flux networks and botnet C2
- Correlate campaigns by shared nameservers

### Key Caveats

- **Temporal Lag:** Passive DNS aggregators may lag 24–48 hours behind real-time resolutions
- **Wildcard Records:** Domains with wildcard DNS records (*.example.com) will pollute reverse lookups
- **Shared Nameservers:** Legitimate ISPs and CDNs manage thousands of domains; shared NS does not indicate malicious intent
- **DNS Caching:** Resolvers cache records; observed resolutions may not match current authoritative responses

---

## Canonical Tools & Resources

### Passive DNS Databases (Query-based)

- **SecurityTrails** (Formerly DNS Trails)
  - **URL:** https://securitytrails.com
  - **Category Path:** `core/dns/passive-dns`
  - **Best For:** Domain/IP reverse lookups, subdomain enumeration, historical DNS with 10+ year coverage
  - **Access:** Web + API
  - **API Availability:** Yes (with subscription)
  - **Signup Required:** Yes
  - **Cost Tier:** Freemium (limited lookups free; paid for advanced queries)
  - **License Type:** Proprietary
  - **Commercial Use:** Allowed (check ToS for tier)
  - **Data Sources:** Aggregates from multiple passive DNS providers; continuous scanning
  - **Limitations:** API rate limits on free tier; coverage gaps for some historical periods
  - **Update Cadence:** Daily to hourly (depending on tier)
  - **Confidence:** High—transparent data source attribution; widely used by SOC/DFIR teams

- **CIRCL Passive DNS** (Community-driven)
  - **URL:** https://www.circl.lu/services/passive-dns/
  - **Category Path:** `core/dns/passive-dns`
  - **Best For:** Non-commercial, research-oriented passive DNS queries; high uptime; community trust
  - **Access:** Web + API
  - **API Availability:** Yes (free, community access)
  - **Signup Required:** No (basic API access); optional registration for features
  - **Cost Tier:** Free
  - **License Type:** OSS
  - **Commercial Use:** Restricted—intended for research/non-profit use
  - **Data Sources:** Community submissions, ISP partnerships, academic networks
  - **Limitations:** Coverage varies by region; less comprehensive than commercial alternatives
  - **Update Cadence:** Real-time (as data submitted)
  - **Confidence:** High—trusted European CERT organization; transparent operations

- **Passive DNS / Farsight Security** (via SecurityTrails API)
  - **URL:** https://securitytrails.com/ (integrates Farsight)
  - **Category Path:** `core/dns/passive-dns`
  - **Best For:** Enterprise-scale passive DNS with 20+ year history
  - **Access:** API (web dashboard via SecurityTrails)
  - **API Availability:** Yes (paid subscription required)
  - **Signup Required:** Yes
  - **Cost Tier:** Paid
  - **License Type:** Proprietary
  - **Commercial Use:** Allowed
  - **Data Sources:** ISP taps, commercial providers, academic sensors
  - **Limitations:** Enterprise-only pricing; not accessible to freelance researchers
  - **Update Cadence:** Continuous
  - **Confidence:** High—used by law enforcement and government agencies

### DNS Record Queries (Real-time Lookup)

- **dig / nslookup (CLI tools)**
  - **URL:** Built into Linux, macOS, Windows (dig via BIND-utils)
  - **Category Path:** `core/dns/dns-lookups`
  - **Best For:** Real-time authoritative DNS queries; no caching (with +nocmd flag)
  - **Access:** CLI
  - **API Availability:** N/A (command-line tool)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** OSS
  - **Commercial Use:** Allowed (part of standard OS)
  - **Data Sources:** Live authoritative nameservers
  - **Limitations:** Single-shot queries; no historical data; subject to DNS caching
  - **Update Cadence:** Real-time
  - **Confidence:** High—queries official sources directly

  **Example usage:**
  ```bash
  dig example.com +short          # Simple A record lookup
  dig example.com MX +short       # Mail exchanger records
  dig -x 192.0.2.1 +short        # Reverse DNS
  dig @8.8.8.8 example.com +trace # Trace resolution path
  ```

- **DNSDumpster** (Online visual DNS lookup)
  - **URL:** https://dnsdumpster.com
  - **Category Path:** `core/dns/dns-lookups`
  - **Best For:** Subdomain enumeration, visual infrastructure mapping, quick reconnaissance
  - **Access:** Web (free, no signup)
  - **API Availability:** No (web-based only)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** Proprietary
  - **Commercial Use:** Allowed (personal use free)
  - **Data Sources:** DNS records + public data aggregation
  - **Limitations:** No historical data; rate-limited for heavy use
  - **Update Cadence:** Real-time
  - **Confidence:** Medium—useful for overview; verify key findings with authoritative sources

### Subdomain & Wildcard Enumeration

- **Sublist3r** (Subdomain enumeration tool)
  - **URL:** https://github.com/aboul3la/Sublist3r
  - **Category Path:** `core/dns/subdomain-enumeration`
  - **Best For:** Fast, passive subdomain discovery using certificate transparency + search engines
  - **Access:** CLI + Python library
  - **API Availability:** Yes (integrates multiple public APIs)
  - **Signup Required:** No (though some integrated APIs may require keys)
  - **Cost Tier:** Free
  - **License Type:** OSS (GPL)
  - **Commercial Use:** Allowed (with attribution)
  - **Data Sources:** CT logs, search engines (Google, Yahoo, Bing), DNS providers
  - **Limitations:** API rate limits; may miss subdomains not in CT logs or search indexes
  - **Update Cadence:** Real-time (queries live sources)
  - **Confidence:** Medium-High—results should be verified with DNS queries

### DNS Security & Validation

- **DNSSEC Validation Tools** (BIND, unbound)
  - **URL:** https://www.isc.org/bind/ (BIND validator)
  - **Category Path:** `core/dns/dnssec`
  - **Best For:** Validating DNSSEC signatures; detecting DNS spoofing attacks
  - **Access:** CLI + library
  - **API Availability:** N/A (tool-based)
  - **Signup Required:** No
  - **Cost Tier:** Free
  - **License Type:** OSS
  - **Commercial Use:** Allowed
  - **Data Sources:** Live DNS queries with DNSSEC validation
  - **Limitations:** Requires understanding of DNSSEC; not all domains implement DNSSEC
  - **Update Cadence:** Real-time
  - **Confidence:** High—cryptographic validation of DNS authenticity

- **SPF / DKIM / DMARC Analysis** (dmarcian, Agari, etc.)
  - **URL:** https://dmarcian.com (free tier available)
  - **Category Path:** `core/dns/email-authentication`
  - **Best For:** Detecting email spoofing, identifying unauthorized senders, brand protection
  - **Access:** Web + API
  - **API Availability:** Yes (paid tier)
  - **Signup Required:** Yes
  - **Cost Tier:** Freemium
  - **License Type:** Proprietary
  - **Commercial Use:** Allowed
  - **Data Sources:** Live DNS lookups for SPF/DKIM/DMARC records; email header analysis
  - **Limitations:** Only detects misconfiguration; authenticated phishing still possible
  - **Update Cadence:** Real-time
  - **Confidence:** High—queries authoritative sources; cryptographic validation where applicable

---

## DNS Pivots & Investigation Patterns

### Pattern 1: Malware C2 Detection via Passive DNS

**Scenario:** You discover a malicious IP (192.0.2.100). What other domains are communicating with it?

1. Query Passive DNS for `192.0.2.100` reverse lookup → Get list of domains that resolved to this IP
2. Pivot each domain to current WHOIS/registrant → Identify hosting patterns
3. Cluster by registrar, registrant, nameserver → Find infrastructure relationships
4. Cross-reference with threat feeds (VirusTotal, URLhaus) → Confirm malicious domains

**Tools:** SecurityTrails API, CIRCL, DomainTools

### Pattern 2: Fast-Flux Detection via DNS TTL Analysis

**Scenario:** Suspected fast-flux botnet (infrastructure designed to evade takedowns)

1. Query passive DNS history for target domain → Observe TTL values and frequency of IP changes
2. If TTL < 5 minutes AND many IP changes → Likely fast-flux
3. Pivot IPs to ASN ownership → Identify bulletproof hosting patterns
4. Cross-reference with known botnet infrastructure → Confirm attribution

**Tools:** SecurityTrails historical data, CIRCL, BGPView (for ASN analysis)

### Pattern 3: Phishing Campaign Attribution via Shared Nameservers

**Scenario:** Detect lookalike domains used in phishing campaign

1. Identify legitimate domain → Extract nameservers (e.g., ns1.example.com)
2. Enumerate all domains using those NS → Legitimate only
3. Find lookalike domains (e.g., examp1e.com, exmple.com) → Check their NS
4. If lookalike NS ≠ legitimate NS → Separate infrastructure; likely phishing
5. Pivot lookalike NS to other domains → Identify full campaign

**Tools:** SecurityTrails reverse NS lookup, DNSDumpster, Sublist3r

---

## Data Quality & Caveats

| Issue | Impact | Mitigation |
|-------|--------|-----------|
| **Wildcard DNS** | Reverse DNS queries polluted with thousands of subdomains | Filter results; query authoritative NS directly |
| **Cached Results** | Older systems may show stale DNS data | Compare with real-time `dig` queries; verify timestamps |
| **Subdomain Explosion** | Some domains have 100k+ subdomains (CDNs, cloud providers) | Filter by relevance; focus on patterns, not volume |
| **GDPR Privacy** | Some registrars hide DNS admin contacts | Use RDAP; expect privacy proxies for EU domains |
| **DNS Rebinding** | Domain resolves differently from different locations/networks | Query from multiple vantage points; note geographic variation |

---

## Integration with Other Data Sources

**DNS ↔ WHOIS Pivot:**
- Domain (from DNS) → Query WHOIS → Registrant info, registrar
- Registrant info → Find other domains → Build campaign map

**DNS ↔ Certificates Pivot:**
- Domain (from DNS) → Search Certificate Transparency → Find SANs and historical certs
- SANs → Discover related domains → Expand investigation scope

**DNS ↔ IP Intelligence Pivot:**
- IP (from DNS) → Query ARIN RDAP → ASN, netblock, organization
- ASN → List all prefixes → Find other potential C2 hosts

---

## Tools Summary Table

| Tool | Type | Cost | Best Use | Confidence |
|------|------|------|----------|-----------|
| **SecurityTrails** | Aggregator | Freemium | Reverse DNS, historical data, API-driven workflows | High |
| **CIRCL Passive DNS** | Aggregator | Free | Research, non-commercial, community trust | High |
| **DNSDumpster** | Visual Lookup | Free | Quick subdomain mapping, no signup | Medium |
| **Sublist3r** | Enumeration | Free/OSS | Passive subdomain discovery | Medium-High |
| **dig/nslookup** | CLI Lookup | Free | Real-time authoritative queries | High |
| **DNSSEC Tools** | Validation | Free/OSS | DNS authenticity verification | High |
| **dmarcian** | Email Auth | Freemium | SPF/DKIM/DMARC analysis, email spoofing detection | High |

---

## Next Steps

- **[OSINT Foundations →](../foundations/README.md)** — Core methodology and verification.
- **[Reputation & Abuse →](../reputation-abuse-reporting/README.md)** — Investigating malicious infrastructure.
