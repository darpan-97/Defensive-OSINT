# Free Tier Tools Index

**Purpose:** Quick reference for defenders with zero budget constraints. All tools listed here have free access with no signup required or free accounts available.

---

## Quick Reference by Category

### DNS Intelligence (Free)

| Tool | URL | Best For |
|------|---|---|
| **dig / nslookup** | CLI (built-in) | Real-time authoritative DNS queries |
| **CIRCL Passive DNS** | https://www.circl.lu/services/passive-dns/ | Non-commercial DNS history; research use |
| **DNSDumpster** | https://dnsdumpster.com | Quick subdomain mapping; visual overview |
| **Sublist3r** | https://github.com/aboul3la/Sublist3r | Passive subdomain discovery via CT + search |

### IP Reputation & Geolocation (Free)

| Tool | URL | Best For |
|------|---|---|
| **AbuseIPDB** | https://www.abuseipdb.com | IP abuse scoring; brute-force detection |
| **Project Honey Pot** | https://www.projecthoneypot.org | Email harvester/spammer tracking |

### Threat Intelligence & Reputation (Free)

| Tool | URL | Best For |
|------|---|---|
| **VirusTotal** | https://www.virustotal.com | Multi-engine file/URL/domain reputation |
| **URLhaus** | https://urlhaus.abuse.ch | Malware distribution URL tracking |
| **PhishTank** | https://www.phishtank.com | Phishing URL database (verified) |
| **abuse.ch Suite** | https://abuse.ch | Malware hashes, C2 IPs, SSL abuse |
| **Spamhaus** | https://www.spamhaus.org | Botnet C&C, malware, phishing blocklists |

---

## Notes on "Free Tier" Definition

**Free (No Signup):** Fully functional without account creation
- Example: dig, crt.sh, BGPView

**Free (Signup Required):** Account creation free; full access free (may have rate limits)
- Example: AbuseIPDB (1000 requests/day free), PhishTank (registration required)

**Freemium (Limited Free):** Free tier with limited functionality; paid tier for advanced features
- Example: VirusTotal (4 requests/min free)

---

## Free Tool Automation & Integration

### Command-Line Examples

```bash
# Query ARIN RDAP for IP ownership
curl https://rdap.arin.net/registry/ip/192.0.2.1

# Reverse DNS lookup (passive)
dig -x 192.0.2.1

# Check Spamhaus DROP list (download)
curl https://www.spamhaus.org/drop/drop.txt
```

---

## Next Steps

- **Learn OSINT methodology?** See [OSINT Foundations](../../core/foundations/)
- **Back to Main?** See [README](../README.md)
