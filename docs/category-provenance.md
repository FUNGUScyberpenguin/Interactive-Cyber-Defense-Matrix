# Control → Category → Vendor mapping: where each part comes from

The tool chains three things together:

    control  →  market category  →  vendor

Each link rests on different ground, and this file says which. It exists because
"is this category real or did we make it up?" is a fair question with a mixed
answer, and a client-facing tool should not blur the two.

## Verification status

| Mark | Meaning |
|---|---|
| ✅ | Confirmed against a published source during this work, cited below |
| ○ | From general knowledge of the analyst landscape — plausible, **not** re-verified against a current Gartner/Forrester subscription |

Anything marked ○ should be checked before it goes in front of a client. Analyst
markets get renamed, merged and retired constantly.

## Tiers

| Tier | Meaning |
|---|---|
| **MQ** | Has, or recently had, a Gartner Magic Quadrant |
| **MG** | Gartner Market Guide, or a Forrester Wave |
| **HC** | Gartner Hype Cycle entry — emerging, sample vendors named, no MQ |
| **TERM** | Widely used industry term with no formal analyst market behind it |
| **FIX** | Coined or mis-named here. Replace with the canonical name given |

---

## Corrections needed

These are the entries that do not survive scrutiny. Everything in this table is
wrong in the shipped code and should change.

| Control | Current category | Problem | Canonical name |
|---|---|---|---|
| App Detection (ADR) | `Runtime Application Security` | Invented phrasing. The category itself is real ✅ | **Application Detection and Response (ADR)** |
| Data Detection (DDR) | `Database Activity Monitoring` | Two different things conflated. DDR is a *capability inside DSPM*, not a market ✅; DAM is a separate legacy market | **Data Detection and Response (DDR)**, noted as a DSPM capability |
| Network Containment | `Network Security` | Far too broad to be a market | **Network Firewalls** ○ |
| WAF & API Protection | `WAF` | Superseded — Gartner folded WAF and API protection together | **WAAP** ○ |
| OT / IoT Security | `OT Security` | Renamed by Gartner | **CPS Protection Platforms** ○ |
| Asset Inventory | `IT Asset Management` | ITAM is an ITOM market, not security | **CAASM** ○ |
| Network Mapping | `Network Discovery` | Not a market | **CAASM** ○ |
| App & API Discovery | `API Discovery` | Discovery is part of the wider market | **API Security** ○ |
| Endpoint Containment | `Endpoint Detection and Response` | Duplicate of the `EDR` entry under a longer name | **EDR** |
| App Remediation | `Virtual Patching` | A WAAP capability, not a market | **WAAP** ○ |
| Crisis Mgmt & Comms | `Breach Response` | Services engagement, not a product market | **Incident Response Services** ○ |
| Endpoint Recovery · App & Config Restore · Immutable Backup | three separate strings | All one market | **Enterprise Backup and Recovery** ○ |
| Policy & Standards · GRC & Assurance | `Policy Management` / `GRC Platform` | Same market | **IT Risk Management (IRM)** ○ |
| Data Breach Response | `Breach Notification` | Legal/notification service | *(leave empty — not a product purchase)* |
| Network Restoration | `Network Configuration Management` | An ITOM market (NCCM), not security | *(leave empty)* |
| Directory Recovery | `Active Directory Recovery` | Capability; vendors sit in ITDR and backup | **TERM** — keep, but labelled as such |
| Detection Engineering & Hunting | `Threat Hunting` | A practice, not a market | **TERM** — keep, but labelled as such |
| Data Discovery | `Data Classification` | A DSPM capability | **TERM** — keep, but labelled as such |

## Categories that hold up

| Category | Tier | Status |
|---|---|---|
| DSPM | MG | ✅ Gartner published its inaugural DSPM Market Guide in September 2025 |
| Application Detection and Response (ADR) | HC | ✅ Gartner Hype Cycle for Application Security names sample vendors; RASP is characterised as legacy |
| CNAPP · SIEM · MDR · EPP · PAM · IGA · Access Management · SASE · Email Security · Application Security Testing · Enterprise Backup and Recovery · Network Firewalls · DRaaS · Business Continuity | MQ | ○ |
| SSPM · ZTNA · Microsegmentation · DDoS Mitigation · DLP · NDR · ITDR · SOAR · Digital Risk Protection · Threat Intelligence · Vulnerability Assessment · PTaaS · TPRM · Cyber Risk Quantification · Patch Management · Enterprise Key Management | MG | ○ |
| EDR | MQ | ○ Folded into the EPP Magic Quadrant, still universal as a term |
| Attack Surface Management | MG | ○ Umbrella over EASM and CAASM, which are the actual Market Guides |
| Multi-Factor Authentication | TERM | ○ Gartner's market is **User Authentication**; MFA is the term buyers use |
| Security Awareness Training | MQ | ○ Gartner renamed this to **Security Behavior and Culture Programs (SBCP)** |
| Cyber Insurance | — | An insurance market, not a security product market |
| vCISO Services · Zero Trust | — | Deliberately empty. Advisory and architectural, not a product purchase |

## Where the vendor half comes from

There is **no free authoritative register** of which vendor sells into which
category. The comprehensive sources are commercial:

- **[IT-Harvest](https://it-harvest.com/)** — 4,000+ vendors and 11,300+ products,
  mapped to NIST CSF 2.0, MITRE ATT&CK and CIS Controls. Subscription. The
  closest thing to the mapping this file wants.
- **Gartner / Forrester** — define the category names the industry uses; vendor
  placement is behind a subscription.

Free and authoritative, but narrow:

- **[NIST NCCoE SP 1800 series](https://www.nccoe.nist.gov/)** — reference builds
  that map *named commercial products* to CSF subcategories. SP 1800-35 (Zero
  Trust Architecture) publishes CSF 2.0 subcategory mappings as spreadsheets.
  Authoritative, but only covers the products in each build.
- **[CIS Controls v8.1 → NIST CSF 2.0](https://www.cisecurity.org/insights/white-papers/cis-controls-v8-1-mapping-to-nist-csf-2-0)**
  — official, free, and covers the control-to-framework link.

Vendors also publish their own: Cisco's *Security Portfolio Mapping to NIST CSF
2.0* is a public worked example of the same exercise.

So `VENDOR_LIBRARY` in `index.html` remains what it says it is — common examples
per category, assembled from general knowledge, not sourced from any register.
Treat it as a starting point and check it before a client sees it.
