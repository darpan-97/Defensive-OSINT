# Free Tier Tools Index

**Purpose:** Quick reference for SOC/DFIR teams with zero budget constraints. All tools listed here have free access with no signup required or free accounts available.

---

## How to Use This Index

1. Find your use-case (SOC, DFIR, etc.)
2. Check which tools have "Free" tier
3. Jump to canonical Core page for full schema details
4. No duplication of full entries; this is a summary index only

---

## Quick Reference by Category

### DNS Intelligence (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **dig / nslookup** | `core/dns/dns-lookups` | CLI (built-in) | Real-time authoritative DNS queries |
| **CIRCL Passive DNS** | `core/dns/passive-dns` | https://www.circl.lu/services/passive-dns/ | Non-commercial DNS history; research use |
| **DNSDumpster** | `core/dns/dns-lookups` | https://dnsdumpster.com | Quick subdomain mapping; visual overview |
| **Sublist3r** | `core/dns/subdomain-enumeration` | https://github.com/aboul3la/Sublist3r | Passive subdomain discovery via CT + search |

### IP Reputation & Geolocation (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **AbuseIPDB** | `core/reputation-abuse-reporting/ip-reputation` | https://www.abuseipdb.com | IP abuse scoring; brute-force detection |
| **Project Honey Pot** | `core/reputation-abuse-reporting/ip-reputation` | https://www.projecthoneypot.org | Email harvester/spammer tracking |

### Threat Intelligence & Reputation (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **VirusTotal** | `core/reputation-abuse-reporting/multi-engine` | https://www.virustotal.com | Multi-engine file/URL/domain reputation |
| **URLhaus** | `core/reputation-abuse-reporting/phishing-malware-urls` | https://urlhaus.abuse.ch | Malware distribution URL tracking |
| **PhishTank** | `core/reputation-abuse-reporting/phishing` | https://www.phishtank.com | Phishing URL database (verified) |
| **abuse.ch Suite** | `core/reputation-abuse-reporting/malware-c2` | https://abuse.ch | Malware hashes, C2 IPs, SSL abuse |
| **Spamhaus** | `core/reputation-abuse-reporting/blocklists` | https://www.spamhaus.org | Botnet C&C, malware, phishing blocklists |

### WHOIS & Domain Registration (Free / Limited Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **ARIN RDAP** | `core/domains-whois-rdap/rdap` | https://rdap.arin.net | IP ownership, ASN allocation (authoritative) |
| **RIPE RDAP** | `core/domains-whois-rdap/rdap` | https://rdap.ripe.net | EU/Middle East IP and domain registration |
| **whois CLI** | `core/domains-whois-rdap/whois` | CLI (built-in) | Raw WHOIS queries; no signup needed |
| **Whois.com (limited)** | `core/domains-whois-rdap/whois` | https://www.whois.com | Web-based WHOIS lookup; limited free queries |

### Certificates & TLS (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **crt.sh** | `core/certificates-ct-tls/ct-search` | https://crt.sh | CT log search; simple interface; no signup |
| **SSLMate (free tier)** | `core/certificates-ct-tls/ct-search` | https://sslmate.com/ct_search_api | CT log aggregator; free basic API access |

### ASN & BGP Analysis (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **BGPView** | `core/asn-bgp-routing/asn-lookup` | https://bgpview.io | ASN lookup, prefix history, peering info |
| **Dan's BGP Lookup** | `core/asn-bgp-routing/asn-lookup` | https://www.dan.me.uk/bgplookup | Live BGP routing table lookup |

### Web Fingerprinting & Asset Discovery (Free)

| Tool | Category Path | URL | Best For |
|------|---|---|---|
| **TheHarvester** | `core/web-fingerprinting-asm/harvesting` | https://github.com/laramies/theHarvester | Email/subdomain discovery; free CLI |
| **favicon.ico Hashing** | `core/web-fingerprinting-asm/favicon` | Via VirusTotal / Shodan | Web app identification via favicon hash |

---

## Free Tools by Use-Case

### SOC Alert Triage (5 min, Free)

1. **VirusTotal** → Quick reputation check
2. **AbuseIPDB** (free tier) → IP abuse scoring
3. **CIRCL Passive DNS** → Domain-IP pivoting
4. **URLhaus** → Malware URL confirmation

**Estimated time:** 3–5 minutes per alert

### DFIR Investigation (Scoping Phase, Free)

1. **ARIN RDAP** → IP ownership
2. **whois CLI** → Domain registration info
3. **CIRCL Passive DNS** → Historical DNS data
4. **crt.sh** → Certificate transparency search
5. **BGPView** → ASN/prefix analysis

**Estimated time:** 30–60 minutes for initial scoping

### Phishing Campaign Tracking (Free)

1. **URLhaus** → Malware distribution URLs
2. **PhishTank** → Verified phishing URLs
3. **WHOIS lookup** → Registrant correlation
4. **crt.sh** → SAN discovery
5. **CIRCL Passive DNS** → Nameserver correlation

**Estimated time:** 45–90 minutes for campaign mapping

---

## Notes on "Free Tier" Definition

**Free (No Signup):** Fully functional without account creation
- Example: dig, crt.sh, BGPView, Dan's BGP Lookup

**Free (Signup Required):** Account creation free; full access free (may have rate limits)
- Example: AbuseIPDB (1000 requests/day free), PhishTank (registration required)

**Freemium (Limited Free):** Free tier with limited functionality; paid tier for advanced features
- Example: VirusTotal (4 requests/min free), SecurityTrails (limited lookups free)

---

## Limitations of Free Tools

- **Rate limits:** Most free tiers have query rate limits (e.g., 1000/day for AbuseIPDB)
- **Historical depth:** Free passive DNS may lack data older than 3–6 months
- **API access:** Some tools restrict API access to paid tiers
- **Real-time updates:** Free tiers may have data lag (e.g., 24–48 hours for passive DNS)
- **Bulk operations:** Difficult to automate at scale with free tools; consider paid tiers for production SOCs

---

## Building a Free OSINT Stack

**Minimum Viable Stack for SOC (Day 1):**
- VirusTotal (reputation)
- AbuseIPDB (IP scoring)
- CIRCL Passive DNS (DNS history)
- WHOIS CLI + ARIN RDAP (ownership)

**Expanded Stack for DFIR (Ongoing):**
- All above +
- crt.sh (certificate search)
- BGPView (ASN analysis)
- URLhaus (malware tracking)
- PhishTank (phishing confirmation)
- Spamhaus blocklists (bulk reputation data)

---

## Free Tool Automation & Integration

### Command-Line Examples

```bash
# Query ARIN RDAP for IP ownership
curl https://rdap.arin.net/registry/ip/192.0.2.1

# Reverse DNS lookup (passive; no automation needed)
dig -x 192.0.2.1

# Check Spamhaus DROP list (download)
curl https://www.spamhaus.org/drop/drop.txt

# Query crt.sh for certificates
curl 'https://crt.sh/?q=example.com&output=json'
```

### SIEM Integration (Splunk, ELK, etc.)

Most free tools either lack API access or have strict rate limits. For production SIEM integration, consider:
- Freemium APIs with higher tier (VirusTotal paid, SecurityTrails paid)
- Self-hosted threat intel platforms (MISP, OpenCTI) + free feed imports
- Community blocklists (Spamhaus) imported via DNS RPZ

---

## Next Steps

- **Want more tools?** See [Freemium Tools Index](../indexes/freemium.md)
- **Ready to invest?** See [Paid Tools Index](../indexes/paid-commercial.md)
- **Learn OSINT methodology?** See [OSINT Foundations](../../core/foundations/)
- **Start SOC triage?** See [SOC Alert Triage](../../use-cases/soc/)
