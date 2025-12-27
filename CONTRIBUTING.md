# Contributing to the Defensive OSINT Repository

Thank you for helping build this resource for the SOC/DFIR community. This document outlines how to contribute resources, maintain quality, and ensure this repository remains a trusted, defensively-focused knowledge base.

---

## Core Principles

Before you contribute, ensure alignment with these principles:

1. **Defensive Only:** All resources must support lawful, defensive security operations
2. **Transparent Sourcing:** Clear attribution and source documentation
3. **Quality Over Quantity:** Depth and accuracy matter more than comprehensive coverage
4. **Community Governance:** Contributions are reviewed for alignment and quality
5. **Legal & Ethical Grounding:** Responsible-use disclaimers and compliance guidance included

---

## Mandatory Resource Schema

Every resource submitted must follow this exact schema. Deviations will be requested before acceptance.

```
- **Name:** [Official tool/resource name]
  - **URL:** [Direct link to official documentation/project page]
  - **Category Path:** [e.g., `core/dns/passive-dns`; must match existing /core/ structure]
  - **Best For:** [1-sentence use case; e.g., "Domain-to-IP reverse lookups with 10+ year history"]
  - **Access:** [Web | API | Both | CLI | Unknown]
  - **API Availability:** [Yes | No | Limited | Unknown]
  - **Signup Required:** [Yes | No | Unknown]
  - **Cost Tier:** [Free | Freemium | Paid]
  - **License Type:** [OSS | Source-available | Proprietary | Unknown]
  - **Commercial Use:** [Allowed | Restricted | Unknown]
  - **Data Sources:** [Where does data come from? e.g., "CT logs, ISP partnerships, scanning"]
  - **Limitations:** [1 critical caveat; e.g., "CDN masking limits origin attribution"]
  - **Update Cadence:** [Real-time | Hourly | Daily | Weekly | Unknown]
  - **Confidence:** [High | Medium | Low] [+ rationale: "Maintained by respected team; transparent operations" or "Academic paper, unverified in practice"]
```

### Schema Field Definitions

| Field | Meaning | Examples |
|-------|---------|----------|
| **Name** | Official project/tool name | SecurityTrails, VirusTotal, dig |
| **URL** | Link to official documentation (not blog posts) | https://securitytrails.com, https://www.virustotal.com |
| **Category Path** | Where in `/core/` structure this belongs | `core/dns/passive-dns`, `core/reputation-abuse-reporting/ip-reputation` |
| **Best For** | What problem does this solve? | "Fast IP reputation checking" not "a tool" |
| **Access** | How do you use it? | Web + API, CLI-only, REST API |
| **API Availability** | Can you automate queries? | Yes, No (web-only), Limited (paid tier only) |
| **Signup Required** | Do you need an account? | No (public access), Yes (even for free tier), Optional |
| **Cost Tier** | Pricing model | Free (no paywall), Freemium (limited free + paid), Paid (requires payment) |
| **License Type** | Under what license is it released? | OSS (e.g., GPL, MIT), Proprietary (closed-source) |
| **Commercial Use** | Can you use this for business? | Allowed, Restricted (non-commercial only), Unknown |
| **Data Sources** | Where does the data originate? | "ISP packet taps + user submissions" |
| **Limitations** | What's the critical weakness? | "High false-positive rate from shared hosting" |
| **Update Cadence** | How current is the data? | Real-time, Daily, Unknown |
| **Confidence** | How reliable is this resource? | High (verified, transparent), Medium (good reputation, some gaps), Low (new, untested) |

---

## Submission Process

### Step 1: Check for Duplicates

Before submitting, search `/core/` for existing entries. If a tool exists:
- **Same category:** Don't duplicate; propose enhancement instead
- **Different category:** Add cross-link under "See also" section (no duplication of full entry)

### Step 2: Prepare Your Submission

Create a Markdown file or GitHub issue with the following:

**File Location:** `submissions/[your-github-handle]_[tool-name].md`

**Format:**

```markdown
# Submission: [Tool Name]

## Resource Details

- **Name:** ...
- **URL:** ...
- **Category Path:** ...
[complete schema above]

## Justification

**Why this tool?**
- Addresses a gap in current coverage: [describe which use-case/workflow]
- Fills category: [e.g., "No free passive DNS tools under /core/dns/"]

**How we verified this:**
- Tested tool: [Yes/No; briefly describe testing]
- Cross-referenced with community: [e.g., "Recommended in r/OSINT 50+ times"]
- Checked ToS: [Confirm compliance with responsible-use guidelines]

## Responsible Use Notes

**Potential Misuse Risk:** [Does this tool have offensive applications? How do we frame defensively?]

**Recommended Disclaimer:** [What safety warning should accompany this tool?]

**Example Use Case (Defensive):**
[Describe a legitimate SOC/DFIR use case]

## References

- [Link to tool documentation]
- [Link to academic paper or case study, if applicable]
- [Community discussion where this was recommended, if applicable]
```

### Step 3: Submit via GitHub

**Option A: Create an Issue**

```
Title: Add [Tool Name] to /core/[category]/

Body:
[Paste complete submission from Step 2]
```

**Option B: Create a Pull Request**

1. Fork the repository
2. Create a branch: `git checkout -b add-[tool-name]`
3. Add your submission file to `/submissions/`
4. Update `/core/[category]/README.md` to include your entry (using schema)
5. Create pull request with description from Step 2

### Step 4: Review & Feedback

Maintainers will review for:

- ✅ **Defensive Framing:** Is this positioned for defense-only use?
- ✅ **Quality Verification:** Have we tested this tool or verified its reputation?
- ✅ **Schema Compliance:** Does it follow the mandatory template?
- ✅ **Duplication Check:** Is this truly new, or does it overlap with existing entries?
- ✅ **Legal/Ethical:** Is responsible-use guidance included?
- ✅ **Accuracy:** Are facts (API availability, cost, updates) correct?

**What to expect:**
- Simple additions: Merged within 1–2 weeks
- Major additions: Possible discussion/requests for modifications
- Declined: Tools that are offensive-focused, unverified, or duplicate existing coverage

---

## Quality Checklist for Reviewers

**Before merging any resource, confirm:**

- [ ] Tool URL is official (not blog post or third-party mirror)
- [ ] **Schema is complete** (no missing fields)
- [ ] **No duplication** (not already listed elsewhere)
- [ ] **Defensive framing:** Responsible-use caveats included if applicable
- [ ] **Data currency:** Is information about cost/access/updates accurate as of submission date?
- [ ] **Community validation:** Can we confirm this tool is actively used and trusted?
- [ ] **Legal compliance:** Does the tool comply with CFAA, GDPR, and other relevant laws?

### Handling Dead Links

- **Quarterly check:** Volunteer maintainers verify all tool URLs
- **Dead link discovered:** Move to `/archive/deprecated-tools.md` with date + reason
- **Replacement tool:** If a replacement exists, note it in the deprecated entry

---

## Deduplication Rules (Mandatory)

**Single Canonical Home:**
- Every tool appears ONCE in Core under primary category
- If a tool fits multiple categories, pick ONE canonical home
- Add "See also" cross-reference pointers (markdown links)
- NO duplication of full schema entries

**Example (Correct):**

Primary location:
```
### SecurityTrails
- [Full schema entry]
```

Secondary category cross-link:
```
#### Related Tools
See also: **SecurityTrails** (`core/dns/passive-dns`) — Also useful for WHOIS pivoting
```

---

## Tagging Convention

All resources must be tagged with at least ONE primary tag:

- **Primary Data Type:** `dns` | `whois` | `asn` | `cert` | `ip-geo` | `web-tech` | `reputation` | `threat-intel` | `automation`
- **Use-Case:** `soc` | `dfir` | `phishing` | `threat-hunting` | `cti` | `asm`
- **Cost Tier:** `free` | `freemium` | `paid`
- **Maturity:** `established` | `active-dev` | `experimental` | `deprecated`

---

## Handling Controversial Tools

### Internet-Wide Scanning (Censys, Shodan, etc.)

These tools can be misused for offensive reconnaissance. When including:

1. **Mandatory disclaimer:** Include responsible-use section
2. **Legal caveat:** Note potential legal implications by jurisdiction
3. **Defensive framing:** Focus on defensive use-cases (asset discovery, exposure hunting)
4. **ToS emphasis:** Highlight tool's ToS restrictions on abuse

### C2/Malware Tracking (URLhaus, abuse.ch, etc.)

Position these defensively:

1. **Framing:** "Defensive tracking of known C&C infrastructure"
2. **Use-cases:** Incident response, threat hunting, detection engineering
3. **NOT covered:** How to operate C2, exploit vulnerabilities, etc.

---

## Community Governance

### Maintenance Team

- **Founder/Lead:** [To be identified]
- **Maintainers:** [Community volunteers with SOC/DFIR experience]
- **Reviewers:** [Vetted security practitioners]

### Decision-Making

- **Tool Additions:** Majority vote (2+ maintainers)
- **Major Structural Changes:** Community discussion required (GitHub discussion board)
- **Deprecated Tools:** Consensus among maintainers + 30-day notice period

### Community Code of Conduct

- Be respectful; disagreement is productive
- Focus on defensive applications
- No harassment, discrimination, or gatekeeping
- Prioritize accessibility (explain jargon, include beginners)

---

## Common Rejection Reasons

Resources will be rejected if:

1. **Offensive framing:** Tool is presented as offensive-focused (e.g., "How to exploit")
2. **Unverified claims:** We can't confirm cost, API availability, or functionality
3. **Duplication:** Tool already exists in repository
4. **Dead/outdated link:** Official project page no longer accessible
5. **Legal gray area:** Tool violates CFAA, GDPR, or other applicable laws without clear disclaimer
6. **Commercial spam:** Submission is promotional material for paid service (must demonstrate value independently)

---

## Questions?

- **Clarification on schema?** Open an issue with `[QUESTION]` tag
- **Discuss governance?** GitHub Discussions board
- **Report a problem?** GitHub Issues

---

## Acknowledgments

Contributors to this repository are acknowledged in the `CONTRIBUTORS.md` file (to be created).

---

**Thank you for making OSINT more accessible to defenders. Together, we build better threat intelligence infrastructure.**
