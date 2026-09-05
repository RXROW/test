# PROJECT OVERVIEW — SecureSist

---

# 1. Document Metadata

| Field | Value |
|---|---|
| **Document name** | `docs/PROJECT_OVERVIEW.md` |
| **Document ID** | D2 |
| **Purpose** | High-level, authoritative overview of the SecureSist product: what it is, who uses it, what it does, what is in and out of scope, and where detailed information lives |
| **Status** | ✅ Active — part of the project Source of Truth |
| **Source phase** | Documentation / Discovery — Gate 2 |
| **Created** | August 2026 |
| **Last reviewed** | August 2026 |
| **Owner** | Backend Engineering |

## 1.1 Source Documents

| ID | Document | Role in this document |
|---|---|---|
| **G0** | `RECONCILIATION_ANALYSIS.md` (Gate 0) | Primary source. Supplies all `R-*`, `I-*`, `M-*`, `X-*`, `W-*` identifiers, the capability inventory, and the entity inventory |
| **G1** | `SCREEN_UI_CHANGE_ANALYSIS.md` (Gate 1) | Supplies the screen inventory, added and removed UI elements |
| **Q1** | `CLIENT_CLARIFICATION_QUESTIONNAIRE.md` | Original clarification questions and structure |
| **A1** | Client answers to Q1 | Explicit client decisions |
| **A2** | Product owner decisions | Explicit internal decisions |
| **A3** | Conflict reconciliation answers | Resolution of 25 identified conflicts |
| **F1** | Current Figma design set (~60 screens) | Design reference for retained screens |
| **X1** | BRD, Feature Breakdown, Frontend Plan, Competitor Comparison, original PRD | Historical context; largely superseded |

## 1.2 Source of Truth Statement

This document is part of an interconnected documentation system. It **owns** product context, scope boundaries, actors, business areas, and the documentation map.

It **does not own** detailed requirements, business rules, domain modelling, architecture, or implementation. Those are owned by the documents identified in §15.

Where information is summarised here, the identifier and the owning document are always stated. **No information from Gate 0 or Gate 1 has been discarded.**

## 1.3 Authority Hierarchy

Applied consistently throughout this document and all downstream documentation:

```
1. Client-confirmed decisions           (A1)
2. Explicit client requirements         (A1)
3. Approved business decisions          (A2, A3)
4. Current Figma / UI behaviour         (F1)
5. Existing documentation               (X1)
6. Existing implementation              (none — greenfield)
7. Engineering inference                (marked INFERRED, never confirmed)
```

---

# 2. Product Summary

## 2.1 What the Product Is

SecureSist is a **unified External Attack Surface Management (EASM) and Threat Intelligence platform**.

It is operated by SecureSist staff on behalf of client organisations. The platform continuously builds a picture of everything about a client organisation that is visible from the public internet, correlates that picture against threat intelligence, evaluates the risk, and produces prioritised, actionable reports.

## 2.2 The Problem It Solves

Organisations do not know what they expose to the internet. Assets are created and forgotten, credentials leak, brands are impersonated, and vulnerabilities appear in systems nobody remembers owning.

Existing tools in the market address one slice of this problem each:

| Competitor | Focus | Gap |
|---|---|---|
| ZeroFox | Social media protection | No infrastructure visibility, no vulnerability intelligence |
| CybelAngel | Data leak detection | No phishing detection, no threat intelligence, no attack surface management |
| ReSecurity | Digital risk monitoring | Limited attack surface discovery, no vulnerability discovery |
| Recorded Future | Threat intelligence feeds | No attack surface discovery, limited brand protection |

**SecureSist's stated differentiator is connecting these slices together.** According to the competitor comparison (source X1), SecureSist is positioned as the only platform offering attack surface discovery, asset discovery, CVE detection, IOC correlation, and advanced dynamic risk scoring in one product.

## 2.3 Primary Users

The platform is used **exclusively by SecureSist internal staff**. Client organisations never log in (`R-EX-01`).

| User | Nature |
|---|---|
| Super Admin | Platform bootstrap; creates Admin accounts |
| Admin | Manages a portfolio of client organisations |
| Security Analyst | Performs the security work for assigned clients |

Client organisations receive value through **reports delivered outside the application** via password-protected links (`R-EX-02`).

## 2.4 Business Purpose

SecureSist sells a managed security service. The platform is the tool its analysts use to deliver that service, and the mechanism by which findings reach the client.

The customer record is therefore a **data partition**, not a user account. Isolation exists to keep analyst work organised and client data separated — not to defend against an external logged-in user.

## 2.5 Core Value

| Value | Delivered by |
|---|---|
| Visibility into unknown internet-facing exposure | Asset discovery across six asset classes |
| Correlation between unrelated signals | Five-dimension clustering and relationship graph |
| Prioritisation that reflects real risk | Two-model scoring (risk and priority) |
| Actionable output | Knowledge-base-backed remediation guidance |
| Response capability | Prepared takedown requests |

## 2.6 Delivery Context

| Aspect | Value | Source |
|---|---|---|
| Product stage | MVP — smallest solution capable of going live | A1 |
| Existing implementation | None (greenfield) | G0 §1.1 |
| Scale target | None specified; conventional web-application design | `R-SC-06` |
| Client base at time of writing | None (pre-launch startup) | A1 |
| Billing / payment processing | **Not applicable.** Contract limits exist as monitoring caps, not as billed quantities. No payment, invoicing, or subscription functionality exists in scope | A1, `R-CU-04` |

---

# 3. Product Scope

## 3.1 Scope Governance Rule

> **The Figma design set is the authoritative reference for what is built** (`R-SC-01`).
> **Anything absent from Figma is excluded, regardless of what earlier documentation states** (`R-SC-02`).
> **Screens removed by explicit decision are removed from both the logic and the Figma set** (`R-SC-03`).

This rule resolves conflict `X-10` and governs every scope statement below.

## 3.2 IN SCOPE

| # | Area | Detail | Requirements |
|:---:|---|---|---|
| 1 | **Authentication** | Login, refresh tokens, account lockout, email invitation | `R-ID-17` → `R-ID-20` |
| 2 | **Super Admin function** | Seed-created account; Admin creation and deletion only | `R-ID-02` → `R-ID-08` |
| 3 | **Admin management** | Invite, delete with mandatory reassignment | `R-ID-09`, `R-ID-22`, `R-ID-23` |
| 4 | **Analyst management** | Invite, delete with mandatory reassignment, assign to customers | `R-ID-09`, `R-ID-11`, `R-ID-12`, `R-ID-21` |
| 5 | **Data isolation** | Admin-level and Analyst-level customer isolation | `R-ID-10`, `R-ID-13`, `R-ID-16` |
| 6 | **Customer management** | Create, edit, activate, soft delete, permanent delete | `R-CU-01` → `R-CU-03`, `R-CU-07`, `R-CU-08` |
| 7 | **Contract management** | Terms, expiry alerting, expiry handling, extension | `R-CU-09` → `R-CU-12` |
| 8 | **Limit management** | Hard enforcement with pending-approval workflow | `R-CU-04` → `R-CU-06` |
| 9 | **Monitoring scope** | Single configuration screen defining what is monitored | `R-CU-13`, `R-CU-14` |
| 10 | **Asset discovery** | Six asset classes, approval workflow, disappearance flagging | `R-AS-01` → `R-AS-06` |
| 11 | **Scanning** | Manual initiation, concurrency control, partial results, retry | `R-SN-01` → `R-SN-05` |
| 12 | **Passive protocol checks** | DNS records, TLS certificates, HTTP headers | `R-SN-06`, `R-SN-07` |
| 13 | **Assessment** | Eight security categories with drill-down | F1 |
| 14 | **Signal ingestion** | Normalisation, aggregation to findings | `R-FD-01`, `R-FD-02` |
| 15 | **Finding approval** | Pending review gate, bulk operations, false-positive marking | `R-FD-03` → `R-FD-07` |
| 16 | **Finding lifecycle** | Four statuses, manual resolution, recurrence tracking | `R-FD-08` → `R-FD-13` |
| 17 | **Severity assignment** | Fixed mapping table maintained in code | `R-FD-14`, `R-FD-15` |
| 18 | **Risk scoring** | Two independent models, four-factor risk score | `R-RS-01` → `R-RS-04` |
| 19 | **Correlation** | Five-dimension clustering with fixed rules | `R-CO-01` → `R-CO-03` |
| 20 | **Investigation** | Four sections: Overview, Graph, Entity, Timeline | `R-CO-04`, `R-CO-05` |
| 21 | **Case management** | Manual creation, three states, finding grouping | `R-CA-01` → `R-CA-07` |
| 22 | **Remediation** | Guidance, internal tracking, manual verification, knowledge base | `R-RM-01` → `R-RM-06` |
| 23 | **Takedown** | Preparation, manual sending, three-day verification, quota | `R-TD-01` → `R-TD-09` |
| 24 | **Reporting** | Two report types, snapshot semantics, template narrative | `R-RP-01` → `R-RP-06` |
| 25 | **Secure delivery** | Password-protected links, web view, PDF download | `R-EX-02` → `R-EX-10` |
| 26 | **Notifications** | Email and in-app, four triggers | `R-NT-01`, `R-NT-02` |
| 27 | **Timeline** | Event history in place of a separate audit log | `R-NT-03` |
| 28 | **Intelligence collection** | Fraud, dark web, surface web, CTI, ransomware | F1, `R-DT-07`, `R-DT-08` |
| 29 | **Connector management** | External provider health, sync, error handling | F1 |
| 30 | **Localisation** | English and Arabic with RTL layout | F1 |

## 3.3 OUT OF SCOPE

| # | Excluded | Reason | Source |
|:---:|---|---|---|
| 1 | Customer portal / customer login | No external access | `R-EX-01` |
| 2 | Partner portal | Not in Figma | `R-SC-02` |
| 3 | Sales pipeline | Not designed | `R-SC-02`, `R-CU-01` |
| 4 | Partner management | Not designed | `R-SC-02`, `R-CU-01` |
| 5 | Demo request handling | Not designed | `R-SC-02`, `R-CU-01` |
| 6 | Internal scheduling / calendar / tasks | Not designed | `R-CU-01` |
| 7 | Module activation console | Not designed | `R-CU-01` |
| 8 | Admin dashboard | Not designed | `R-CU-01` |
| 9 | **Monitor module** | Removed; recurrence tracking retained internally | G0 §6.2, A3 |
| 10 | **Predict module** | Removed; risk-reduction preview retained inside Remediation | G0 §6.2, `R-RM-06` |
| 11 | **VIP Protection module** | Removed | `R-SC-03` |
| 12 | Detection rules interface | Removed; severity from fixed table | `R-FD-16` |
| 13 | Investigate: Actor, Campaign, IOC, Correlation, Workspace tabs | Removed | `R-CO-05` |
| 14 | Saved investigation views (shared) | No sharing between analysts | `R-CO-06` |
| 15 | Investigation templates | Removed | `R-CO-05` |
| 16 | Scheduled reports | Removed | `R-RP-05` |
| 17 | Remediation approval workflow | Removed | `R-RM-04` |
| 18 | Automated validation execution | Removed; verification is manual | `R-RM-03` |
| 19 | Separate audit log | Timeline events only | `R-NT-03` |
| 20 | Two-factor authentication | Removed | `R-ID-18` |
| 21 | Single sign-on | Removed | `R-ID-19` |
| 22 | Self-service signup | Accounts by invitation only | `R-ID-02` |
| 23 | Cross-customer views | No cross-customer visibility | `R-ID-16` |
| 24 | Internal network scanning | External-only product | `R-SC-04` |
| 25 | Third-party / supply chain risk | Not in Figma | `R-SC-02` |
| 26 | SIEM / SOC outbound integration | Not in Figma | `R-SC-02` |
| 27 | Automated takedown sending | Manual sending only | `R-TD-01` |
| 28 | SLA tracking and due dates | No time-based targets | `R-RS-05`, `R-RS-06` |
| 29 | Automatic data deletion | Retention is indefinite until manual deletion | `R-DT-04` |
| 30 | Billing, invoicing, payment processing | Not present in any source | A1 |

## 3.4 PENDING SCOPE

Scope items that are confirmed in principle but whose extent is not finalised. **None of these may be treated as settled.**

| ID | Item | Status | Blocks |
|---|---|---|---|
| **B2** | Scan module dedicated screens | ⚠️ NEEDS CONFIRMATION — the three Scan screens duplicate Assessment almost exactly. `R-SN-01` confirms Scan as a stage but does not confirm dedicated screens | Gate 1 sign-off |
| **B3** | Private (unshared) saved views | ⚠️ NEEDS CONFIRMATION — `R-CO-06` removes *sharing*; whether private views also go is undecided. Affects four existing screens | Gate 1 sign-off |
| **M-10** | Monitoring Scope field list | ⚠️ PENDING SIGN-OFF — screen confirmed, fields proposed but not approved | Gate 1 sign-off, PRD |
| **M-12** | Report section content | ⚠️ PENDING SIGN-OFF — two types confirmed, sections undefined | PRD |
| **M-13** | Profile and Settings content | ⚠️ PENDING SIGN-OFF — in scope but undesigned | PRD |
| — | Clusters tab in Detect & Score | ⚠️ NEEDS CONFIRMATION — clustering confirmed (`R-CO-01`) but Investigate Correlation removed. Whether the Detect & Score Clusters tab survives is unstated | Gate 1 sign-off |
| — | Forgot / reset password flow | ⚠️ NEEDS CONFIRMATION — not explicitly confirmed for Admin and Analyst; Super Admin recovery is by script (`R-ID-08`) | Gate 1 sign-off |

---

# 4. Actors & Roles

Three roles are confirmed (`R-ID-01`). Conflict `X-02` (PRD stated six personas) is resolved: the six were descriptions of *client-side* staff who do not use the system.

---

## 4.1 Super Admin

| Aspect | Detail | Source |
|---|---|---|
| **Responsibility** | Platform bootstrap and Admin account provisioning |
| **Created by** | Seed script; no UI creation path | `R-ID-02` |
| **Count** | One | `R-ID-05` |
| **Access areas** | Admin Management screen only | `R-ID-04` |
| **Major capabilities** | Invite Admin by email · Delete Admin | `R-ID-03`, `R-ID-06` |
| **Access boundaries** | No access to customer data, findings, reports, or any operational screen | `R-ID-04` |
| **Isolation rules** | Not applicable — sees no operational data |
| **Login** | Same login screen as other roles | `R-ID-07` |
| **Password recovery** | By script, not by UI | `R-ID-08` |
| **Related workflows** | `W-01` Platform Bootstrap · `W-02` Admin Onboarding · `W-15` User Deletion |
| **⚠️ Open** | Whether the Super Admin account appears in any user listing | G0 §7.1 |

---

## 4.2 Admin

| Aspect | Detail | Source |
|---|---|---|
| **Responsibility** | Owns a portfolio of client organisations and manages their commercial and configuration lifecycle |
| **Created by** | Super Admin, by email invitation | `R-ID-09` |
| **Access areas** | Customers · Profile · Settings · Analyst Management | `R-CU-01` |
| **Major capabilities** | Create, edit, activate, delete customers · Configure contracts, limits, entitlements · Configure monitoring scope · Create Analyst accounts · Assign one Analyst per customer · Set secure link validity | `R-CU-*`, `R-ID-09`, `R-ID-12`, `R-EX-05` |
| **Access boundaries** | Sees **only** customers they own; other Admins' customers are invisible. Sees customer records and contracts but **not findings** | `R-ID-10`, `R-ID-15` |
| **Isolation rules** | Customer ownership is exclusive per Admin. Analysts are a **shared pool** — all Admins see all Analysts | `R-ID-10`, `R-ID-11` |
| **Related workflows** | `W-03` Analyst Onboarding · `W-04` Customer Onboarding · `W-14` Contract Expiry · `W-15` User Deletion · `W-16` Customer Deletion |
| **⚠️ Open** | Whether an Admin can view another Admin's analyst assignments | G0 §7.2 |
| **⚠️ Open** | Whether an Analyst can modify the Monitoring Scope of an assigned customer, or whether this is Admin-only | G0 §7.3 |

---

## 4.3 Security Analyst

| Aspect | Detail | Source |
|---|---|---|
| **Responsibility** | Performs all security work for assigned client organisations |
| **Created by** | Any Admin, by email invitation | `R-ID-09` |
| **Access areas** | Assessment · Discover & Map · Scan · Investigate · Detect & Score · Report & Prioritize · Profile · Settings | F1 |
| **Major capabilities** | Run scans · Approve or reject discovered assets · Approve or reject findings · Change finding status · Create and manage cases · Track remediation · Verify fixes manually · Prepare and send takedowns · Generate and share reports · Approve limit overruns | `R-SN-*`, `R-AS-04`, `R-FD-*`, `R-CA-*`, `R-RM-*`, `R-TD-*`, `R-RP-*`, `R-CU-05` |
| **Access boundaries** | Sees **only** customers explicitly assigned to them. Cannot create customers, manage contracts, or assign analysts | `R-ID-13`, `R-ID-15` |
| **Isolation rules** | **Exactly one Analyst per customer** (`R-ID-14`). No Analyst sees another Analyst's customers (`R-ID-16`) |
| **Related workflows** | `W-05` → `W-13` (all operational workflows) |

---

## 4.4 Non-Actors

Entities that appear in the design but are **not** system users. Documented to prevent them being modelled as accounts.

| Entity | Nature | Where stored | Source |
|---|---|---|---|
| Client organisation | Data record | `Customer` entity | G0 §7.4 |
| Client contact person | Contact detail | Field within `Customer` | G0 §7.4 |
| Remediation owner (e.g. "Jordan Miles") | Descriptive text | Field within `RemediationItem` | G0 §7.4 |
| Team names in Figma (SOC Queue, IR Team, etc.) | Descriptive labels only | Not modelled | G0 §7.4 |
| Report recipient | Email address on a share link | `ShareLink` entity | G0 §7.4 |

---

## 4.5 Isolation Model Summary

```
Super Admin
    │ creates / deletes
    ▼
Admin ────────── owns an exclusive set of Customers
    │             (no other Admin can see them)
    │ creates
    ▼
Security Analyst ── shared pool, visible to all Admins
    │
    │ assigned by an Admin to specific Customers
    ▼
Customer ──────── visible to exactly one Analyst
                  and to its owning Admin only
```

**Confirmed rules:**

| Rule | Requirement |
|---|---|
| Admin sees only own customers | `R-ID-10` |
| All Admins see all Analysts | `R-ID-11` |
| Admin assigns Analysts only to own customers | `R-ID-12` |
| Analyst sees only assigned customers | `R-ID-13` |
| Exactly one Analyst per customer | `R-ID-14` |
| Admin does not see findings | `R-ID-15` |
| No cross-customer visibility for anyone | `R-ID-16` |

---

# 5. High-Level Business Areas

> **These are preliminary business areas, not the final Domain → Feature Mapping.**
> The formal domain model will be produced in `DOMAIN_FEATURE_MAPPING.md` (D4). Boundaries below may change when that work is done.

Fourteen business areas are identified. Each is described by business purpose, not technical structure.

---

## BA-01 — Platform Administration & Access

| Aspect | Detail |
|---|---|
| **Purpose** | Control who can use the platform and what they can reach |
| **Major capabilities** | Authentication · account lockout · email-based provisioning · Super Admin bootstrap · Admin management · Analyst management · analyst-to-customer assignment · ownership reassignment on deletion · role-based access enforcement · Admin-level isolation · Analyst-level isolation · profile management · session listing and termination (capabilities #1–13) |
| **Roles** | Super Admin · Admin · Security Analyst |
| **Workflows** | `W-01` · `W-02` · `W-03` · `W-15` |
| **Entities** | User · Role · Permission · Session · RefreshToken · Invitation |
| **UI areas** | Login · Set Password · Forgot Password ⚠️ · Admin Management · Analyst Management · Profile · Settings |
| **Requirements** | `R-ID-01` → `R-ID-23` |
| **Boundary note** | Stable. Unlikely to change during domain modelling |

---

## BA-02 — Client Organisation Management

| Aspect | Detail |
|---|---|
| **Purpose** | Manage the commercial and configuration record of each client organisation |
| **Major capabilities** | Customer creation and editing · contract management · module entitlement management · limit configuration · usage tracking · limit enforcement with pending approval · activation · contract expiry handling · soft deletion · permanent deletion · search and filtering · directory export (capabilities #14–26) |
| **Roles** | Admin |
| **Workflows** | `W-04` · `W-07` · `W-14` · `W-16` |
| **Entities** | Customer · Contract · ModuleEntitlement · MonitoringLimit · UsageCounter |
| **UI areas** | Customer Directory · Tenant Configuration · Customer Detail |
| **Requirements** | `R-CU-01` → `R-CU-12` |
| **Boundary note** | Stable |

---

## BA-03 — Monitoring Scope Definition

| Aspect | Detail |
|---|---|
| **Purpose** | Define what the system watches on behalf of each client |
| **Major capabilities** | Monitoring scope configuration (capability #24) |
| **Roles** | Admin (⚠️ Analyst access unconfirmed) |
| **Workflows** | Part of `W-04` |
| **Entities** | MonitoringScope |
| **UI areas** | Monitoring Scope (NEW — not yet designed) |
| **Requirements** | `R-CU-13` · `R-CU-14` |
| **Boundary note** | ⚠️ Separated from BA-02 deliberately. This area supplies the input data on which BA-05, BA-06, and BA-07 depend. Field list pending (`M-10`) |

---

## BA-04 — External Attack Surface Discovery

| Aspect | Detail |
|---|---|
| **Purpose** | Find everything belonging to the client that is visible from the internet |
| **Major capabilities** | External asset discovery · asset classification across six classes · functional asset typing · asset approval workflow · disappearance detection · criticality assignment · technology fingerprinting · inventory browsing · bulk operations · watchlist management (capabilities #27–36) |
| **Roles** | Security Analyst |
| **Workflows** | `W-06` |
| **Entities** | Asset · Service · Certificate · DNSRecord · TechnologyStack |
| **UI areas** | Discover & Map (Overview · Attack Surface · Inventory · Asset Detail) |
| **Requirements** | `R-AS-01` → `R-AS-06` |
| **Boundary note** | Stable |

---

## BA-05 — Security Scanning & Assessment

| Aspect | Detail |
|---|---|
| **Purpose** | Test discovered assets for weaknesses |
| **Major capabilities** | Manual scan initiation · concurrency control · multi-stage execution · partial-failure handling and retry · progress reporting · port and service detection · vulnerability matching · web application checks · TLS and certificate analysis · DNS configuration analysis · misconfiguration detection · assessment across eight categories · coverage tracking · snapshots (capabilities #37–50) |
| **Roles** | Security Analyst |
| **Workflows** | `W-05` |
| **Entities** | ScanRun · ScanResult · AssessmentSnapshot · AssessmentCoverage |
| **UI areas** | Assessment (9 screens) · Scan (3 screens ⚠️ retention unconfirmed) |
| **Requirements** | `R-SN-01` → `R-SN-08` |
| **Boundary note** | ⚠️ `R-SN-01` confirms Scan and Assessment as two stages. Whether they remain two UI areas is pending (`B2`) |

---

## BA-06 — Threat & Exposure Intelligence

| Aspect | Detail |
|---|---|
| **Purpose** | Gather external intelligence about threats affecting the client |
| **Major capabilities** | Reputation and blacklist checking · threat feed ingestion · dark web signal collection · leaked credential detection · surface web exposure detection · phishing domain detection · brand abuse detection · fake application detection · ransomware leak-site monitoring · ransomware early-warning indicators · IOC management · threat actor tracking · campaign tracking (capabilities #51–63) |
| **Roles** | Security Analyst |
| **Workflows** | Feeds `W-08` |
| **Entities** | IOC · ThreatActor · Campaign · IntelligenceFeed · Signal |
| **UI areas** | Discover & Map (Fraud · Dark Web · Surface · Threat/CTI tabs) |
| **Requirements** | `R-DT-01` · `R-DT-05` · `R-DT-07` · `R-DT-08` |
| **Boundary note** | ⚠️ Whether IOC, ThreatActor and Campaign are shared across clients or held per client is unresolved (G0 §10.6) |

---

## BA-07 — Detection & Risk Evaluation

| Aspect | Detail |
|---|---|
| **Purpose** | Convert raw signals into evaluated, prioritised findings |
| **Major capabilities** | Signal normalisation · deduplication · signal-to-finding aggregation · severity assignment from mapping table · confidence aggregation · finding approval workflow · status management · recurrence detection · false-positive marking · risk score calculation · priority score calculation · score explanation · score history (capabilities #64–76) |
| **Roles** | Security Analyst |
| **Workflows** | `W-08` · `W-09` |
| **Entities** | Signal · Finding · Evidence · SeverityMapping · RiskScore · PriorityScore · ScoreSnapshot |
| **UI areas** | Detect & Score · Pending Review (NEW) |
| **Requirements** | `R-FD-01` → `R-FD-17` · `R-RS-01` → `R-RS-06` |
| **Boundary note** | ⚠️ This is the product's core differentiator. Parameter values pending: `M-01` · `M-02` · `M-03` · `M-06` |

---

## BA-08 — Correlation & Investigation

| Aspect | Detail |
|---|---|
| **Purpose** | Connect related findings so analysts see problems, not isolated rows |
| **Major capabilities** | Relationship derivation · five-dimension clustering · graph traversal and expansion · entity detail aggregation · timeline event recording · timeline browsing · investigation workspace · saved investigation views ⚠️ (capabilities #77–84) |
| **Roles** | Security Analyst |
| **Workflows** | Supports `W-09` · `W-10` |
| **Entities** | Relationship · Cluster · TimelineEvent |
| **UI areas** | Investigate (Overview · Graph · Entity · Timeline) |
| **Requirements** | `R-CO-01` → `R-CO-06` |
| **Boundary note** | ⚠️ Capability #84 (saved views) status depends on `B3`. Clustering threshold pending (`M-05`) |

---

## BA-09 — Incident Response

| Aspect | Detail |
|---|---|
| **Purpose** | Group related findings into a single managed incident |
| **Major capabilities** | Case creation and management · finding-to-case linking (capabilities #85–86) |
| **Roles** | Security Analyst |
| **Workflows** | `W-10` |
| **Entities** | Case |
| **UI areas** | Case views within Investigate and Detect & Score |
| **Requirements** | `R-CA-01` → `R-CA-07` |
| **Boundary note** | Stable |

---

## BA-10 — Remediation Guidance

| Aspect | Detail |
|---|---|
| **Purpose** | Tell the client what to fix and track whether it was fixed |
| **Major capabilities** | Remediation guidance retrieval · knowledge base management · progress tracking · manual verification · risk reduction calculation (capabilities #87–91) |
| **Roles** | Security Analyst |
| **Workflows** | `W-11` |
| **Entities** | RemediationItem · KBArticle |
| **UI areas** | Remediation Guidance · Finding Intelligence |
| **Requirements** | `R-RM-01` → `R-RM-06` |
| **Boundary note** | SecureSist advises; the client implements. Actual implementation is a separate commercial service outside this platform (`R-RM-01`). Knowledge base content pending (`M-11`) |

---

## BA-11 — Content Takedown

| Aspect | Detail |
|---|---|
| **Purpose** | Remove malicious content impersonating or targeting the client |
| **Major capabilities** | Takedown preparation · evidence collection · status tracking · removal verification · quota enforcement (capabilities #92–96) |
| **Roles** | Security Analyst |
| **Workflows** | `W-12` |
| **Entities** | TakedownRequest |
| **UI areas** | Takedown Queue · Takedown Detail (both NEW) |
| **Requirements** | `R-TD-01` → `R-TD-09` |
| **Boundary note** | The system prepares; the analyst sends manually (`R-TD-01`). Verification runs every three days while the customer is active (`R-TD-06`) — this is the only recurring background job in the product |

---

## BA-12 — Reporting & Client Delivery

| Aspect | Detail |
|---|---|
| **Purpose** | Package findings into documents the client receives |
| **Major capabilities** | Report data assembly · template-based narrative generation · snapshot storage · secure link generation · password generation and delivery · link revocation · public report rendering · CSV export · evidence pack assembly (capabilities #97–105) |
| **Roles** | Security Analyst |
| **Workflows** | `W-13` |
| **Entities** | Report · ShareLink · EvidencePack |
| **UI areas** | Report & Prioritize · Public Report View (NEW, unauthenticated) |
| **Requirements** | `R-RP-01` → `R-RP-06` · `R-EX-01` → `R-EX-10` |
| **Boundary note** | This is the only area that produces output visible outside the organisation. Report section content pending (`M-12`) |

---

## BA-13 — Notifications & Activity History

| Aspect | Detail |
|---|---|
| **Purpose** | Surface events requiring attention and record what happened |
| **Major capabilities** | Alert generation · email delivery · in-app notification (capabilities #111–113); timeline event recording is shared with BA-08 |
| **Roles** | Admin · Security Analyst |
| **Workflows** | Cross-cutting |
| **Entities** | Alert · Notification · TimelineEvent |
| **UI areas** | Notification Panel (NEW) · Timeline within Investigate |
| **Requirements** | `R-NT-01` → `R-NT-03` |
| **Boundary note** | Four fixed alert triggers, not configurable (`R-NT-02`). Recipient mapping is inferred, not confirmed (`I-03`, `M-14`) |

---

## BA-14 — External Data Provider Management

| Aspect | Detail |
|---|---|
| **Purpose** | Manage the external services the product depends on for all its data |
| **Major capabilities** | Connector management · credential storage · health monitoring · sync error handling · rate limit management (capabilities #106–110) |
| **Roles** | Admin (⚠️ role assignment not explicitly confirmed) |
| **Workflows** | Supports `W-05` · `W-08` |
| **Entities** | Connector |
| **UI areas** | Discover & Map → Integrations tab |
| **Requirements** | `R-DT-01` → `R-DT-03` |
| **Boundary note** | ⚠️ Provider selection unresolved (`M-08`). Names shown in Figma are placeholders (`R-DT-03`) |

---

## 5.1 Cross-Cutting Capabilities

Three capabilities belong to no single business area:

| # | Capability | Used by |
|:---:|---|---|
| 114 | Scheduled job execution | BA-02 (expiry), BA-11 (verification) |
| 115 | Long-running job tracking | BA-04, BA-05, BA-12 |
| 116 | Global search | BA-02, BA-04, BA-07, BA-08 |

---

# 6. Major Features / Capabilities

The full inventory of **116 capabilities** is owned by Gate 0 §8. This section maps them to business areas and status. No capability has been dropped.

| # Range | Capability group | Business Area | Roles | Requirement IDs | Workflow IDs | Status |
|---|---|---|---|---|---|---|
| 1–13 | Access & administration | BA-01 | All | `R-ID-01` → `R-ID-23` | `W-01`, `W-02`, `W-03`, `W-15` | **CONFIRMED** |
| 14–26 | Customer operations | BA-02 | Admin | `R-CU-01` → `R-CU-12` | `W-04`, `W-07`, `W-14`, `W-16` | **CONFIRMED** |
| 24 | Monitoring scope configuration | BA-03 | Admin | `R-CU-13`, `R-CU-14` | `W-04` | **CONFIRMED** (fields ⚠️ `M-10`) |
| 27–36 | Discovery & inventory | BA-04 | Analyst | `R-AS-01` → `R-AS-06` | `W-06` | **CONFIRMED** |
| 32 | Asset criticality assignment | BA-04 | Analyst | `R-AS-06` | — | **CONFIRMED** (rules ⚠️ `M-04`) |
| 37–50 | Scanning & assessment | BA-05 | Analyst | `R-SN-01` → `R-SN-08` | `W-05` | **CONFIRMED** |
| 51–63 | Intelligence collection | BA-06 | Analyst | `R-DT-01`, `R-DT-05`, `R-DT-07`, `R-DT-08` | Feeds `W-08` | **CONFIRMED** (providers ⚠️ `M-08`) |
| 64–72 | Detection | BA-07 | Analyst | `R-FD-01` → `R-FD-17` | `W-08`, `W-09` | **CONFIRMED** |
| 67 | Severity assignment | BA-07 | — | `R-FD-14`, `R-FD-15` | `W-08` | **CONFIRMED** (table ⚠️ `M-03`) |
| 68 | Confidence aggregation | BA-07 | — | `R-RS-04` | `W-08` | **CONFIRMED** (weights ⚠️ `M-06`) |
| 73–76 | Scoring | BA-07 | Analyst | `R-RS-01` → `R-RS-06` | — | **CONFIRMED** (weights ⚠️ `M-01`, `M-02`) |
| 77–83 | Correlation & investigation | BA-08 | Analyst | `R-CO-01` → `R-CO-05` | `W-09`, `W-10` | **CONFIRMED** |
| 78 | Five-dimension clustering | BA-08 | — | `R-CO-01`, `R-CO-02`, `R-CO-03` | — | **CONFIRMED** (threshold ⚠️ `M-05`) |
| 84 | Saved investigation views | BA-08 | Analyst | `R-CO-06` | — | ⚠️ **PENDING** (`B3`) |
| 85–86 | Case management | BA-09 | Analyst | `R-CA-01` → `R-CA-07` | `W-10` | **CONFIRMED** |
| 87–91 | Remediation | BA-10 | Analyst | `R-RM-01` → `R-RM-06` | `W-11` | **CONFIRMED** (KB content ⚠️ `M-11`) |
| 92–96 | Takedown | BA-11 | Analyst | `R-TD-01` → `R-TD-09` | `W-12` | **CONFIRMED** |
| 97–105 | Output & delivery | BA-12 | Analyst | `R-RP-01` → `R-RP-06`, `R-EX-01` → `R-EX-10` | `W-13` | **CONFIRMED** (sections ⚠️ `M-12`, default validity ⚠️ `M-09`) |
| 106–110 | Connector management | BA-14 | Admin | `R-DT-01` → `R-DT-03` | Supports `W-05`, `W-08` | **CONFIRMED** (providers ⚠️ `M-08`) |
| 111–113 | Notifications | BA-13 | Admin, Analyst | `R-NT-01`, `R-NT-02` | Cross-cutting | **CONFIRMED** (recipients ⚠️ `M-14`) |
| 114–116 | Platform services | Cross-cutting | — | Implied by `R-TD-06`, `R-CU-09` | Multiple | **CONFIRMED** |

**Detailed capability behaviour will be owned by `FEATURE_BREAKDOWN.md`.**

---

# 7. Major Workflows

Sixteen workflows are identified. Full detail — including undefined behaviour — is owned by Gate 0 §9 and will be elaborated in `PRD.md` and `BUSINESS_RULES.md`.

| ID | Workflow | Trigger | Actor | High-level steps | End state | Business Area |
|---|---|---|---|---|---|---|
| **W-01** | Platform Bootstrap | Deployment | System / operator | Seed script creates Super Admin → credentials delivered out of band | Super Admin can log in | BA-01 |
| **W-02** | Admin Onboarding | Super Admin invites | Super Admin | Enter email → invitation sent → recipient sets password | Admin active | BA-01 |
| **W-03** | Analyst Onboarding | Admin invites | Admin | Enter email → invitation sent → recipient sets password | Analyst available for assignment | BA-01 |
| **W-04** | Customer Onboarding | Admin creates customer | Admin | Details → limits → contract → modules → monitoring scope → save (Pending) → assign Analyst → activate | Customer Active | BA-02, BA-03 |
| **W-05** | Manual Scan | Analyst initiates | Analyst | Check running scan → block if present → execute stages → collect results → mark failures → auto-retry | Completed · Partial · Failed | BA-05 |
| **W-06** | Asset Approval | Discovery produces asset | System, then Analyst | Classify → auto-accept if subdomain of confirmed domain → otherwise queue → approve or reject | Monitored · Excluded | BA-04 |
| **W-07** | Limit Overrun | Discovery exceeds limit | System, then Analyst | Discovery continues → new items pending → Analyst approves or rejects → Admin notified | Approved or excluded | BA-02 |
| **W-08** | Signal to Finding | Signal received | System, then Analyst | Normalise → deduplicate → attach or create finding → assign severity → calculate confidence → Pending Review → approve or reject | Open · False Positive | BA-06, BA-07 |
| **W-09** | Finding Lifecycle | Finding approved | Analyst | Open → In Review → (Monitoring) → Resolved | Resolved | BA-07 |
| **W-10** | Case Handling | Analyst creates case | Analyst | Create → attach findings → Investigating → Closed | Closed | BA-09 |
| **W-11** | Remediation Tracking | Finding requires fix | Analyst | Retrieve KB article → present guidance → include in report → client implements externally → mark steps → verify manually → set Resolved | Finding Resolved | BA-10 |
| **W-12** | Takedown | Malicious content identified | System, then Analyst | Collect evidence → identify recipient → prepare draft → Analyst reviews → Analyst sends externally → record response → verify every 3 days → close | Completed · Rejected | BA-11 |
| **W-13** | Report Generation & Delivery | Analyst generates report | Analyst | Select type and scope → assemble data → store snapshot → render → create secure link → generate password → email customer contact | Link active until expiry or revocation | BA-12 |
| **W-14** | Contract Expiry | Scheduled daily check | System | Detect end date within 30 days → alert Admin → on expiry set Not Active → disable scanning → retain data | Not Active, or Active if extended | BA-02 |
| **W-15** | User Deletion | Admin or Super Admin deletes user | Admin / Super Admin | Identify owned customers → require replacement selection → transfer ownership → deactivate account | Account removed, customers reassigned | BA-01 |
| **W-16** | Customer Deletion | Admin deletes customer | Admin | Soft delete → hidden from listings → data retained → optional manual permanent deletion | Deleted or permanently removed | BA-02 |

## 7.1 Undefined Workflow Behaviour

Recorded in Gate 0 §9 and preserved here. **These are gaps, not decisions.**

| Workflow | Undefined behaviour | Owner |
|---|---|---|
| `W-01` | Super Admin password recovery beyond re-running the script | `BUSINESS_RULES.md` |
| `W-02`, `W-03` | Invitation expiry period | `BUSINESS_RULES.md` |
| `W-04` | Whether monitoring scope is mandatory before activation | `BUSINESS_RULES.md` |
| `W-05` | Minimum interval between scans | `BUSINESS_RULES.md` |
| `W-06` | Whether rejected assets can be reinstated | `BUSINESS_RULES.md` |
| `W-07` | Whether limit approval is one-time or permanent | `BUSINESS_RULES.md` |
| `W-08` | Deduplication matching criteria | `BUSINESS_RULES.md` |
| `W-09` | Conditions for entering Monitoring status (`I-07`) | `BUSINESS_RULES.md` |
| `W-10` | Whether a closed case can be manually reopened | `BUSINESS_RULES.md` |
| `W-11` | How the client communicates remediation progress | `PRD.md` |
| `W-12` | Verification method for non-domain targets (apps, social accounts) | `BUSINESS_RULES.md` |
| `W-13` | Where the rendered document is stored | `ARCHITECTURE.md` |
| `W-14` | Whether existing reports remain accessible after expiry | `BUSINESS_RULES.md` |
| `W-15` | Whether Timeline entries retain a deleted user's name | `BUSINESS_RULES.md` |
| `W-16` | Whether permanent deletion requires additional confirmation | `BUSINESS_RULES.md` |

---

# 8. High-Level Entity Overview

Forty-five business entities are identified. This is **not a data model** — no fields, no collections, no indexes.

> **ID note:** Gate 0 catalogued entities by name and group without assigning `ENT-*` identifiers. Formal `ENT-*` IDs will be assigned by `DOMAIN_FEATURE_MAPPING.md` (D4). Until then, entities are traceable by name and by their Gate 0 group reference (G0 §10.1 – §10.10).

---

## 8.1 Identity Entities — G0 §10.1

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **User** | Account for one of the three roles | Role · assigned Customers | Invited → Active → Deleted | BA-01 | Password reset flow ⚠️ |
| **Role** | Grouping of permissions | Permissions | Static | BA-01 | — |
| **Permission** | A single capability grant | Roles | Static | BA-01 | Complete list not enumerated |
| **Session** | An active login | User | Created → Expired / Revoked | BA-01 | Retention period |
| **RefreshToken** | Session continuation credential | User · Session | Issued → Used → Revoked | BA-01 | Duration (`M-07`) |
| **Invitation** | A pending, unaccepted account | Inviting User | Sent → Accepted → Expired | BA-01 | Expiry period |

## 8.2 Client Organisation Entities — G0 §10.2

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Customer** | The client organisation being monitored | Owning Admin · assigned Analyst · all operational data | Draft → Pending → Active → Not Active → Deleted | BA-02 | — |
| **Contract** | Commercial terms and validity period | Customer | Active → Expired → Renewed | BA-02 | — |
| **ModuleEntitlement** | Which product modules the client has purchased | Customer | Enabled / Disabled | BA-02 | Module list after VIP removal |
| **MonitoringLimit** | Contractual caps on assets, domains, takedowns | Customer | Static per contract | BA-02 | — |
| **UsageCounter** | Consumption against limits | Customer · MonitoringLimit | Continuous | BA-02 | Takedown quarter reset timing |
| **MonitoringScope** | Definition of what the system watches | Customer | Created → Updated | BA-03 | Field list (`M-10`) |

## 8.3 Asset Entities — G0 §10.3

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Asset** | A discovered internet-facing item | Customer · parent Asset · Services · Findings | Discovered → Pending / Monitored → Flagged deleted | BA-04 | — |
| **Service** | A port and protocol observed on an asset | Asset | Observed → Changed | BA-04 | — |
| **Certificate** | A TLS certificate belonging to an asset | Asset | Valid → Expiring → Expired | BA-04 | — |
| **DNSRecord** | A DNS entry for an asset | Asset | Active → Missing → Dangling | BA-04 | — |
| **TechnologyStack** | Software detected on an asset | Asset | Observed | BA-04 | — |

## 8.4 Scanning Entities — G0 §10.4

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **ScanRun** | One execution of a scan | Customer · ScanResults | Queued → Running → Completed / Partial / Failed | BA-05 | — |
| **ScanResult** | Raw technical output before interpretation | ScanRun · Asset | Produced → Interpreted | BA-05 | — |
| **AssessmentSnapshot** | Point-in-time state of the assessment | Customer | Captured | BA-05 | Capture frequency without scheduling (`I-02`) |
| **AssessmentCoverage** | Record of what was checked, including clean results | ScanRun | Recorded | BA-05 | — |

## 8.5 Detection Entities — G0 §10.5

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Signal** | A raw observation from one provider | Customer · Finding · Source | Received → Attached | BA-06, BA-07 | Deduplication criteria |
| **Finding** | A confirmed security issue | Customer · Asset · Signals · Case | Pending → Open → In Review → Monitoring → Resolved | BA-07 | Monitoring usage (`I-07`) |
| **Evidence** | Proof supporting a finding | Finding · Signal | Captured | BA-07 | — |
| **SeverityMapping** | Reference table mapping signal type to severity | Reference data | Static | BA-07 | Contents (`M-03`) |
| **RiskScore** | Measure of how dangerous a finding is | Finding | Calculated → Recalculated | BA-07 | Weights (`M-01`) |
| **PriorityScore** | Measure of what should be worked first | Finding | Calculated | BA-07 | Weights (`M-02`) |
| **ScoreSnapshot** | Historical record of score changes | Finding | Recorded | BA-07 | — |

## 8.6 Intelligence Entities — G0 §10.6

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **IOC** | A technical indicator of compromise | Signals · Actors · Campaigns | Observed → Verified | BA-06 | ⚠️ Shared across clients or per client |
| **ThreatActor** | An attacker group | Campaigns · IOCs | Tracked | BA-06 | ⚠️ Shared across clients or per client |
| **Campaign** | A coordinated attack effort | Actors · IOCs · Victims | Active → Dormant | BA-06 | ⚠️ Shared across clients or per client |
| **IntelligenceFeed** | A connection to an external provider | Signals | Healthy → Warning → Failed | BA-06, BA-14 | — |

## 8.7 Correlation Entities — G0 §10.7

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Relationship** | A link between two entities | Two entities | Created → Updated | BA-08 | — |
| **Cluster** | A group of related findings | Findings · shared entities | Formed → Updated | BA-08 | Threshold (`M-05`) |

## 8.8 Response Entities — G0 §10.8

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Case** | A grouped incident | Findings · Analyst | Open → Investigating → Closed | BA-09 | Manual reopening |
| **RemediationItem** | Tracking of a fix in progress | Finding · KBArticle | Not started → In progress → Verified | BA-10 | — |
| **KBArticle** | Reusable remediation guidance | Findings | Published → Updated | BA-10 | Inventory (`M-11`) |
| **TakedownRequest** | A request to remove malicious content | Signal · Evidence · Customer | Draft → Ready → Sent → Acknowledged → Completed / Rejected | BA-11 | Non-domain verification method |

## 8.9 Output Entities — G0 §10.9

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Report** | A generated document | Customer · Findings | Generated → Shared → Expired | BA-12 | Storage location (`W-13`) |
| **ShareLink** | Password-protected external delivery | Report | Created → Active → Expired / Revoked | BA-12 | Default validity (`M-09`) |
| **EvidencePack** | Bundled proof for a finding | Finding · Evidence | Assembled → Downloaded | BA-12 | — |

## 8.10 Platform Entities — G0 §10.10

| Entity | Business purpose | Major relationships | Lifecycle | Business Area | Unresolved |
|---|---|---|---|---|---|
| **Connector** | Link to an external data provider | Signals | Connected → Degraded → Failed | BA-14 | Provider list (`M-08`) |
| **JobRun** | A long-running operation | Subject entity | Queued → Running → Completed / Failed | Cross-cutting | — |
| **TimelineEvent** | A recorded occurrence | Customer · subject entity | Recorded | BA-08, BA-13 | Which events are recorded (`I-11`) |
| **Alert** | A notification instance | Trigger · recipient | Active → Acknowledged → Resolved | BA-13 | Recipients (`M-14`) |
| **Notification** | A delivery record | Alert · User | Sent → Read | BA-13 | — |

**Detailed entity modelling is owned by `DOMAIN_FEATURE_MAPPING.md` (D4) and `ARCHITECTURE.md` (D6).**

---

# 9. Screen & UI Summary

> Full detail is owned by **`SCREEN_UI_CHANGE_ANALYSIS.md` (Gate 1)**. This section summarises structure only.
>
> **ID note:** Gate 1 did not assign `UI-*` identifiers. Screens are traceable by name and Gate 1 section reference. Formal `UI-*` IDs will be assigned in `PRD.md` (D3).

## 9.1 Application Structure

```
UNAUTHENTICATED
  └── Login
  └── Set Password (invitation)
  └── Forgot Password ⚠️
  └── Public Report View          ← the only client-facing surface

SUPER ADMIN
  └── Admin Management            ← only screen available

ADMIN
  ├── Customer Directory
  ├── Tenant Configuration
  ├── Customer Detail
  ├── Monitoring Scope            ← NEW, not yet designed
  ├── Analyst Management          ← NEW
  ├── Profile
  └── Settings

SECURITY ANALYST
  ├── Workspace Dashboard
  ├── Assessment                  (9 screens)
  ├── Discover & Map              (9 tabs + 3 graph views)
  ├── Scan                        (3 screens ⚠️ retention pending)
  ├── Investigate                 (4 tabs: Overview · Graph · Entity · Timeline)
  ├── Detect & Score
  ├── Pending Review              ← NEW
  ├── Report & Prioritize
  ├── Takedown Queue / Detail     ← NEW
  ├── Notification Panel          ← NEW
  ├── Profile
  └── Settings
```

## 9.2 New Screens Required — 13

Screens confirmed by approved requirements that **do not exist in Figma**.

| Screen | Roles | Business Area | Gate 1 § |
|---|---|---|---|
| Login | All | BA-01 | §1.1 |
| Set Password | Admin, Analyst | BA-01 | §1.2 |
| Forgot / Reset Password ⚠️ | Admin, Analyst | BA-01 | §1.3 |
| Admin Management | Super Admin | BA-01 | §1.4 |
| Analyst Management | Admin | BA-01 | §1.5 |
| **Monitoring Scope** | Admin | BA-03 | §1.6 |
| Pending Review | Analyst | BA-07 | §1.7 |
| Takedown Queue | Analyst | BA-11 | §1.8 |
| Takedown Detail | Analyst | BA-11 | §1.8 |
| Public Report View | External (no login) | BA-12 | §1.9 |
| Profile ⚠️ | All | BA-01 | §1.10 |
| Settings ⚠️ | All | BA-01 | §1.10 |
| Notification Panel | Admin, Analyst | BA-13 | §1.11 |

> **Monitoring Scope is the highest-priority new screen.** Without it, four modules have no input data. Each field group enables a specific capability (Gate 1 §1.6).

## 9.3 Added UI Elements — 14

Additions to screens that already exist in Figma. Full detail in Gate 1 §2.

| Element | Screen | Requirement |
|---|---|---|
| Monitoring Scope access point | Tenant Configuration | `R-CU-13` |
| Analyst assignment field (required) | Tenant Configuration | `R-ID-14`, `R-CU-08` |
| Assigned Analyst column | Customer Directory | `R-ID-14` |
| Analyst filter | Customer Directory | `R-ID-12` |
| Permanent delete action | Customer Detail | `R-CU-03` |
| Contract extension action | Customer Detail | `R-CU-12` |
| Pending approval state | Asset Inventory | `R-AS-04` |
| Deleted indicator | Asset Inventory, Asset Detail | `R-AS-05` |
| Limit overrun pending state | Asset Inventory, Customer Detail | `R-CU-05` |
| False positive status | Findings List | `R-FD-06` |
| Recurrence indicator | Findings List, Finding Detail | `R-FD-11` |
| Supporting signals section | Finding Detail | `R-FD-01`, `R-FD-12` |
| Report sharing dialog | Report screens | `R-EX-02` → `R-EX-06` |
| Language toggle | All screens | F1 |

## 9.4 Removed Screens — 16

> ⚠️ **These exist in Figma and must NOT be built.**

| Removed | Replaced by | Requirement |
|---|---|---|
| Detection Rules / Signals View | Fixed severity table in code | `R-FD-16` |
| Investigate: Actor View | Actors as graph nodes | `R-CO-05` |
| Investigate: Campaign View | Campaigns as graph nodes | `R-CO-05` |
| Investigate: IOC / Signal Mapping | IOCs as graph nodes | `R-CO-05` |
| Investigate: Correlation View | Clustering surfaced elsewhere ⚠️ | `R-CO-05` |
| Investigate: Workspace View | Nothing | `R-CO-05` |
| Saved Investigation Views | Nothing | `R-CO-06` |
| Investigation Templates | Nothing | `R-CO-05` |
| Monitor module | Recurrence counter on findings | G0 §6.2 |
| Predict module | Risk Reduction Preview in Remediation | `R-RM-06` |
| VIP Protection module | Nothing | `R-SC-03` |
| Admin: Dashboard | Nothing | `R-CU-01` |
| Admin: Sales | Nothing | `R-CU-01` |
| Admin: Partners | Nothing | `R-CU-01` |
| Admin: Demo Requests | Nothing | `R-CU-01` |
| Admin: Modules | Nothing | `R-CU-01` |
| Admin: Schedule | Nothing | `R-CU-01` |

## 9.5 Removed UI Elements — 14

> ⚠️ **These appear inside retained screens and must NOT be built.** Full detail in Gate 1 §4.

| Element | Screens affected | Requirement |
|---|---|---|
| SLA fields and due dates | Prioritization Queue, Remediation, Findings List | `R-RS-05` |
| Remediation approval drawer | Finding Intelligence | `R-RM-04` |
| Automated validation execution | Finding Intelligence, Remediation | `R-RM-03` |
| Scheduled Reports metric | Export / Reporting Entry | `R-RP-05` |
| Cross-customer scope value | Findings List, Alerts Center | `R-ID-16` |
| Saved Views ⚠️ | Findings, Assessment, Investigate, Vulnerability Results | `R-CO-06` |
| Alert rules configuration | Alerts Center | `R-NT-02` |
| Automated takedown sending | Fraud Intelligence, Domain Detail | `R-TD-01` |
| Audit log | Not designed | `R-NT-03` |
| Three-stage provisioning display | Customer Detail | `R-CU-07` |
| Continuous discovery messaging | Discover & Map, Assessment | `R-SN-02` |
| Two-factor authentication | Not designed | `R-ID-18` |
| Single sign-on | Not designed | `R-ID-19` |
| Self-service signup | Not designed | `R-ID-02` |

> **Clarification on validation:** the Validation Checklist **remains** as a manual checklist. Only the automated execution is removed.

## 9.6 Figma Divergence

> ⚠️ **The current Figma set no longer matches the approved scope.** It contains 16 removed screens and 14 removed elements. Until Figma is updated, Gate 1 is the authoritative reference for what to build.

---

# 10. External Integrations

Every piece of security data comes from an external provider (`R-DT-01`). **No provider has been selected** (`M-08`).

| Integration | Business purpose | Status | Requirements | Pending |
|---|---|---|---|---|
| **Asset discovery** | Find subdomains and internet-facing systems | ⚠️ PENDING provider | `R-AS-01` | `M-08` |
| **Port and service data** | Identify exposed services | ⚠️ PENDING provider | `R-SN-01` | `M-08` |
| **Vulnerability data** | Match software versions to known vulnerabilities | ⚠️ PENDING provider | `R-FD-15` | `M-08` |
| **Reputation and blacklists** | Check assets against abuse lists | ⚠️ PENDING provider | `R-DT-01` | `M-08` |
| **Dark web intelligence** | Detect leaked data and criminal activity | ⚠️ PENDING provider and budget | `R-DT-05` | `M-08`, `Q28` |
| **Phishing detection** | Detect fraudulent lookalike domains | ⚠️ PENDING provider | `R-DT-01` | `M-08` |
| **Fake application detection** | Detect impersonating mobile apps | ⚠️ PENDING provider | `R-AS-01` | `M-08` |
| **Ransomware leak sites** | Detect the client named as a victim | ⚠️ PENDING provider | `R-DT-07` | `M-08` |
| **Geographic IP data** | Map server locations | ⚠️ PENDING provider | F1 | `M-08` |
| **Email delivery** | Send report links, passwords, and alerts | ⚠️ PENDING provider | `R-EX-04`, `R-NT-01` | `M-08` |
| **File storage** | Store screenshots, evidence, reports | ⚠️ PENDING provider | `R-RP-04` | `M-08` |
| **Hosting** | Run the application | ⚠️ PENDING | — | `M-08` |

## 10.1 Confirmed Integration Constraints

| Constraint | Source |
|---|---|
| All security data via external APIs | `R-DT-01` |
| Lowest cost prioritised; expensive services excluded | `R-DT-02` |
| Provider names appearing in Figma are placeholders | `R-DT-03` |
| Passive protocol lookups permitted directly (DNS, TLS, HTTP headers) | `R-SN-06` |
| No active or intrusive scanning under any circumstance | `R-SN-07` |

## 10.2 Ownership

> `M-08` blocks `INTEGRATIONS.md` (D7) only. **It does not block any other document.**
> Provider evaluation options were catalogued in `CLIENT_CLARIFICATION_QUESTIONNAIRE.md` §21 and will be finalised in D7.

---

# 11. Important Product Constraints

Constraints are client-approved product boundaries, not technical assumptions.

## 11.1 Access & Isolation Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-01 | Only SecureSist internal staff have accounts | `R-EX-01` |
| C-02 | There is no self-service signup; all accounts are created by invitation | `R-ID-02` |
| C-03 | Each Admin sees only the customers they own | `R-ID-10` |
| C-04 | Each Analyst sees only the customers assigned to them | `R-ID-13` |
| C-05 | Exactly one Analyst per customer | `R-ID-14` |
| C-06 | Admins do not see findings | `R-ID-15` |
| C-07 | No role has cross-customer visibility | `R-ID-16` |
| C-08 | Super Admin has no operational data access | `R-ID-04` |
| C-09 | Deleting a user requires reassigning their customers first | `R-ID-21`, `R-ID-22`, `R-ID-23` |

## 11.2 Security Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-10 | The product is external-only; no internal network scanning | `R-SC-04` |
| C-11 | No active or intrusive scanning is performed | `R-SN-07` |
| C-12 | Only passive protocol lookups are permitted | `R-SN-06` |
| C-13 | The customer contract constitutes authorisation to monitor | `R-SN-08` |
| C-14 | Two-factor authentication is not implemented | `R-ID-18` |
| C-15 | Account lockout after five failed attempts | `R-ID-20` |

## 11.3 Data Handling Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-16 | Leaked credential data stores email addresses only, never passwords | `R-DT-05` |
| C-17 | Exposed file evidence stores location and type only, never content | `R-DT-06` |
| C-18 | No data is deleted automatically; retention is indefinite until manual deletion | `R-DT-04` |
| C-19 | Rejected findings are marked False Positive, never deleted | `R-FD-06` |
| C-20 | Assets that disappear are flagged, never removed | `R-AS-05` |
| C-21 | Customer deletion is soft by default; permanent deletion is a separate manual action | `R-CU-02`, `R-CU-03` |
| C-22 | Customer data is retained after contract expiry | `R-CU-11` |
| C-23 | No personal contact data for client executives is stored | A1, `R-SC-03` |

## 11.4 Workflow Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-24 | Scanning is manual only; no scheduled or continuous scanning | `R-SN-02` |
| C-25 | Concurrent scans for the same customer are blocked | `R-SN-03` |
| C-26 | Customer activation is instantaneous with no background provisioning | `R-CU-07` |
| C-27 | Activation is blocked unless an Analyst is assigned | `R-CU-08` |
| C-28 | Contract limits are hard-enforced with a pending-approval path | `R-CU-04`, `R-CU-05` |
| C-29 | Discovery continues when a limit is reached; only new items enter pending | `R-CU-06` |
| C-30 | Findings require Analyst approval before entering the active queue | `R-FD-03` |
| C-31 | Resolved status is set manually, never automatically | `R-FD-09` |
| C-32 | Resolved findings reopen automatically on re-detection | `R-FD-10` |
| C-33 | Cases are created manually only; the system does not suggest them | `R-CA-02` |
| C-34 | A finding belongs to at most one case | `R-CA-04` |
| C-35 | Closing a case does not change its findings' status | `R-CA-05` |
| C-36 | SecureSist advises on remediation; implementation is an external service | `R-RM-01` |
| C-37 | Fix verification is manual | `R-RM-03` |
| C-38 | The system prepares takedown requests; the Analyst sends them | `R-TD-01` |
| C-39 | Takedown retries count against the quarterly quota | `R-TD-08` |
| C-40 | No SLA or time-based targets exist anywhere in the product | `R-RS-05`, `R-RS-06` |

## 11.5 Output Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-41 | Reports are a snapshot taken at generation time, never live | `R-RP-04` |
| C-42 | Report narrative is template-generated, not AI-generated | `R-RP-06` |
| C-43 | Secure links are password-protected, one password per link | `R-EX-02`, `R-EX-03` |
| C-44 | The system sends the password to the customer contact on record | `R-EX-04` |
| C-45 | Links can be revoked before expiry | `R-EX-06` |
| C-46 | No access tracking and no view limit on links | `R-EX-07` |
| C-47 | The link opens a web page, not a file | `R-EX-08` |
| C-48 | The backend returns data; the frontend renders the document | `R-RP-03` |

## 11.6 Product Constraints

| ID | Constraint | Requirement |
|---|---|---|
| C-49 | The Figma design set defines what is built | `R-SC-01` |
| C-50 | Anything absent from Figma is excluded | `R-SC-02` |
| C-51 | Removed screens are deleted from Figma as well as from the logic | `R-SC-03` |
| C-52 | There is no user-editable detection rules interface | `R-FD-16` |
| C-53 | Alert triggers are fixed at four; they are not configurable | `R-NT-02` |
| C-54 | Timeline Events replace a separate audit log | `R-NT-03` |
| C-55 | Investigations are not shared between team members | `R-CO-06` |
| C-56 | The delivery target is an MVP capable of going live | `R-SC-05` |
| C-57 | No scale target is specified; conventional design applies | `R-SC-06` |
| C-58 | No billing, invoicing, or payment functionality exists | A1 |

---

# 12. Important Confirmed Decisions

> Gate 0 recorded decisions as requirements (`R-*`) rather than separate `DEC-*` identifiers. The `R-*` ID is therefore the decision's stable identifier. No `DEC-*` IDs are invented here.

## 12.1 Scope Decisions

| Decision | ID |
|---|---|
| Figma is the authoritative scope reference | `R-SC-01` |
| Anything absent from Figma is out of scope | `R-SC-02` |
| Removed items are deleted from Figma too | `R-SC-03` |
| The product is external-only | `R-SC-04` |
| Delivery target is a live-capable MVP | `R-SC-05` |
| Monitor module removed; recurrence tracking retained internally | G0 §6.2, A3 |
| Predict module removed; risk reduction preview retained | `R-RM-06`, A3 |
| VIP Protection removed | `R-SC-03` |
| Admin Console reduced to Customers, Profile, Settings | `R-CU-01` |

## 12.2 Access Decisions

| Decision | ID |
|---|---|
| Three roles: Super Admin, Admin, Security Analyst | `R-ID-01` |
| Super Admin is seed-created and limited to Admin provisioning | `R-ID-02`, `R-ID-03` |
| Admin-level customer isolation | `R-ID-10` |
| Analysts are a shared pool visible to all Admins | `R-ID-11` |
| One Analyst per customer | `R-ID-14` |
| Admins do not see findings | `R-ID-15` |
| No cross-customer visibility | `R-ID-16` |
| No two-factor authentication, no single sign-on | `R-ID-18`, `R-ID-19` |

## 12.3 Client Access Decisions

| Decision | ID |
|---|---|
| No customer portal and no partner portal | `R-EX-01` |
| Reports delivered by password-protected secure link | `R-EX-02` |
| Password generated automatically and emailed by the system | `R-EX-03`, `R-EX-04` |
| Link validity configured by the Admin | `R-EX-05` |
| Links are revocable; no tracking, no view limit | `R-EX-06`, `R-EX-07` |
| Link opens a web page with a PDF download option | `R-EX-08`, `R-EX-09` |

## 12.4 Operational Decisions

| Decision | ID |
|---|---|
| Scan and Assessment are two separate stages | `R-SN-01` |
| Manual scanning only | `R-SN-02` |
| Passive protocol lookups permitted with a clear boundary | `R-SN-06`, `R-SN-07` |
| Contract is sufficient authorisation | `R-SN-08` |
| Multiple signals may support one finding | `R-FD-01` |
| Findings require Analyst approval | `R-FD-03` |
| Four finding statuses | `R-FD-08` |
| Severity from a fixed table in code; no rules interface | `R-FD-14`, `R-FD-16` |
| Two independent scoring models | `R-RS-01` |
| Four risk factors following conventional practice | `R-RS-02`, `R-RS-03` |
| No SLA or time targets | `R-RS-05` |
| Five clustering dimensions with a shared threshold, fixed in code | `R-CO-01` → `R-CO-03` |
| Investigate reduced to four sections | `R-CO-05` |
| Cases created manually only | `R-CA-02` |
| Remediation is advisory; verification is manual | `R-RM-01`, `R-RM-03` |
| Knowledge base of reusable articles | `R-RM-05` |
| Takedown prepared by system, sent by Analyst | `R-TD-01` |
| Takedown verification every three days while customer is active | `R-TD-06` |

## 12.5 Data Decisions

| Decision | ID |
|---|---|
| All security data via external APIs | `R-DT-01` |
| Lowest cost prioritised | `R-DT-02` |
| Figma provider names are placeholders | `R-DT-03` |
| Retention indefinite until manual deletion | `R-DT-04` |
| Leaked credentials: email addresses only | `R-DT-05` |
| Exposed files: location and type only | `R-DT-06` |
| Ransomware: leak sites plus early warning | `R-DT-07`, `R-DT-08` |
| Timeline Events instead of an audit log | `R-NT-03` |

---

# 13. Pending Decisions

**None of the items below may be treated as resolved.** Gate 0 classified all 14 missing decisions as parameter, reference-data, or content items — **no structural decision remains open**.

## 13.1 Parameter Values — Block `BUSINESS_RULES.md` (D5)

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **M-01** | Numeric weights for the four Risk Score factors | Every risk figure in the product depends on them | ⚠️ PENDING SIGN-OFF |
| **M-02** | Priority Score factor weights | Determines the order analysts work in | ⚠️ PENDING SIGN-OFF |
| **M-05** | Clustering threshold value | Wrong value produces either no clusters or meaningless ones | ⚠️ PENDING SIGN-OFF |
| **M-06** | Provider trust weights for confidence aggregation | Required by `R-RS-04` | ⚠️ PENDING SIGN-OFF |
| **M-07** | Access and refresh token durations | Confirmed as "conventional"; values not stated | ⚠️ PENDING SIGN-OFF |
| **M-09** | Default secure link validity period | Admin configures it; the default is unspecified | ⚠️ PENDING SIGN-OFF |

## 13.2 Reference Data — Blocks `BUSINESS_RULES.md` (D5)

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **M-03** | Signal type → severity mapping table (~30–50 entries) | Replaces the removed rules engine; without it findings have no severity | ⚠️ PENDING SIGN-OFF |
| **M-04** | Asset type → criticality tier mapping | Feeds one of the four risk factors | ⚠️ PENDING SIGN-OFF |
| **M-14** | Alert recipient mapping | Currently inferred (`I-03`), not confirmed | ⚠️ NEEDS CONFIRMATION |

## 13.3 Content — Resolvable While Writing `PRD.md` (D3)

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **M-10** | Monitoring Scope screen field list | Each field enables a specific capability; omitting one silently disables it | ⚠️ PENDING SIGN-OFF |
| **M-12** | Report section content for the two report types | Two types confirmed; sections undefined | ⚠️ PENDING SIGN-OFF |
| **M-13** | Profile and Settings screen content | In scope but undesigned | ⚠️ PENDING SIGN-OFF |

## 13.4 Content — Non-Blocking

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **M-11** | Knowledge base article inventory | Approach confirmed; articles must be authored by security staff | ⚠️ PENDING — content work, not documentation work |

## 13.5 Provider Selection — Blocks `INTEGRATIONS.md` (D7) only

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **M-08** | Final external provider list | The product cannot function without data providers | ⚠️ PENDING SIGN-OFF |

## 13.6 Gate 1 Blockers

| ID | Description | Why it matters | Status |
|---|---|---|---|
| **B1** | Monitoring Scope field list | Same as `M-10` | ⚠️ PENDING SIGN-OFF |
| **B2** | Whether Scan has dedicated screens | Binary decision affecting three screens and the analyst navigation | ⚠️ NEEDS CONFIRMATION |
| **B3** | Whether private saved views remain | Affects four existing screens, each with a metric, a table, and two actions | ⚠️ NEEDS CONFIRMATION |

## 13.7 Lower-Impact Open Questions

| Description | Affected area | Status |
|---|---|---|
| Forgot / reset password flow for Admin and Analyst | BA-01 | ⚠️ NEEDS CONFIRMATION |
| Whether "customers assigned" count is global or per-Admin | BA-01 | ⚠️ NEEDS CONFIRMATION |
| Whether the Clusters tab survives in Detect & Score | BA-08 | ⚠️ NEEDS CONFIRMATION |
| Whether an Analyst can edit Monitoring Scope | BA-03 | ⚠️ NEEDS CONFIRMATION |
| Whether an Admin sees another Admin's analyst assignments | BA-01 | ⚠️ NEEDS CONFIRMATION |
| Whether the Super Admin appears in user listings | BA-01 | ⚠️ NEEDS CONFIRMATION |
| Whether IOC / ThreatActor / Campaign are shared or per-customer | BA-06 | ⚠️ NEEDS CONFIRMATION |

## 13.8 Blocking Summary

| Document | Blocked by |
|---|---|
| `PROJECT_OVERVIEW.md` (D2) | **Nothing** |
| `PRD.md` (D3) | `M-10` · `M-12` · `M-13` · `B2` · `B3` — resolvable during writing |
| `DOMAIN_FEATURE_MAPPING.md` (D4) | **Nothing** |
| `BUSINESS_RULES.md` (D5) | `M-01` · `M-02` · `M-03` · `M-04` · `M-05` · `M-06` · `M-07` · `M-09` · `M-14` |
| `ARCHITECTURE.md` (D6) | **Nothing** |
| `INTEGRATIONS.md` (D7) | `M-08` |
| `FEATURE_BREAKDOWN.md` (D8) | Depends on D3, D5 |

---

# 14. Confirmed vs Inferred vs Pending

This distinction is preserved throughout the document and must be preserved in all downstream documentation.

## 14.1 CONFIRMED

Explicitly supported by client requirements, client decisions, or approved product-owner decisions.

| Category | Count | ID Range |
|---|:---:|---|
| Requirements | 104 | `R-SC-*` · `R-ID-*` · `R-CU-*` · `R-EX-*` · `R-AS-*` · `R-SN-*` · `R-FD-*` · `R-RS-*` · `R-CO-*` · `R-CA-*` · `R-RM-*` · `R-TD-*` · `R-RP-*` · `R-NT-*` · `R-DT-*` |
| Workflows | 16 | `W-01` → `W-16` |
| Roles | 3 | §4 |
| Business areas | 14 | `BA-01` → `BA-14` |
| Capabilities | 116 | G0 §8 |
| Entities | 45 | G0 §10 |
| Constraints | 58 | `C-01` → `C-58` |
| Resolved conflicts | 24 | `X-01` → `X-24` |

## 14.2 INFERRED — NOT CONFIRMED

Logical consequences of confirmed decisions. **These are not requirements.** Owned by Gate 0 §3.

| ID | Inference | Needs validation |
|---|---|---|
| **I-01** | Background jobs exist despite "manual scanning only" | Confirm that "manual" constrains scanning, not all automation |
| **I-02** | The Assessment cycle is triggered by a manual scan, not continuously | Confirm the Assessment screen reflects the last manual run |
| **I-03** | Alert recipients follow role boundaries | Confirm recipient mapping (`M-14`) |
| **I-04** | Findings carry the owning Admin's identity through the customer | Confirm Admin isolation extends to finding queries |
| **I-05** | Removing detection rules means severity must be authored as reference data | The table itself must be produced (`M-03`) |
| **I-06** | The Pending Review queue is per-customer and visible only to the assigned Analyst | Confirm |
| **I-07** | Monitoring status is for accepted-but-watched findings | Confirm intended usage |
| **I-08** | Report generation requires an active customer | Confirm whether reports can be generated for expired customers |
| **I-09** | The secure link renders data server-side | Confirm the link page is a separate public view |
| **I-10** | Settings and Profile contain minimal functionality | Confirm expected content (`M-13`) |
| **I-11** | Timeline Events are written by application logic at each state change | Confirm which events are recorded |
| **I-12** | Asset criticality rules derive from asset type | The rule set must be produced (`M-04`) |

> **Note on `I-01`:** confirmation of `R-TD-06` (three-day takedown verification) and `R-CU-09` (contract expiry alerting) means recurring background jobs **do** exist. The inference is therefore strongly supported but the boundary — which automation is permitted — has not been stated explicitly.

## 14.3 PENDING / NEEDS DECISION

| Category | Count | IDs |
|---|:---:|---|
| Parameter values | 6 | `M-01` · `M-02` · `M-05` · `M-06` · `M-07` · `M-09` |
| Reference data | 3 | `M-03` · `M-04` · `M-14` |
| Content | 4 | `M-10` · `M-11` · `M-12` · `M-13` |
| Provider selection | 1 | `M-08` |
| Gate 1 blockers | 3 | `B1` · `B2` · `B3` |
| Lower-impact questions | 7 | §13.7 |
| Superseded conflicts | 1 | `X-25` |

---

# 15. Documentation Map

| Document | ID | Purpose | Information owned | Status |
|---|---|---|---|---|
| `RECONCILIATION_ANALYSIS.md` | G0 | Requirements reconciliation | All `R-*`, `I-*`, `M-*`, `X-*`, `W-*` IDs · capability inventory · entity inventory · source hierarchy | ✅ Approved |
| `SCREEN_UI_CHANGE_ANALYSIS.md` | G1 | Screen and UI change reference | Screen inventory · required UI data · added and removed UI elements | ⚠️ Pending `B1`, `B2`, `B3` |
| **`PROJECT_OVERVIEW.md`** | **D2** | **High-level product overview** | **Product context · scope · actors · business areas · documentation map · traceability rules** | **✅ This document** |
| `PRD.md` | D3 | Detailed requirements | Functional and non-functional requirements · screen-level specifications · acceptance criteria · `UI-*` ID assignment | ⬜ Not started |
| `DOMAIN_FEATURE_MAPPING.md` | D4 | Business structure | Domains · subdomains · features · entity model · `ENT-*` ID assignment · state machines | ⬜ Not started |
| `BUSINESS_RULES.md` | D5 | Business constraints | Numbered rules (`BR-*`) · validation · scoring formulas · severity mapping · clustering thresholds · permission matrix · error behaviour | ⬜ Not started |
| `ARCHITECTURE.md` | D6 | Technical design | Layering · module boundaries · data flow · job execution · integration pattern · isolation enforcement · decision records | ⬜ Not started |
| `INTEGRATIONS.md` | D7 | External dependencies | Provider inventory · purpose · limits · failure handling · credential storage · sync cadence | ⬜ Blocked by `M-08` |
| `FEATURE_BREAKDOWN.md` | D8 | Feature behaviour | Feature-level specifications · edge cases | ⬜ Not started |
| `API_CONTRACT.yaml` | D9 | Interface agreement | OpenAPI specification · endpoints · schemas · error codes | ⬜ Not started |
| `DELIVERY_PLAN.md` | D10 | Work sequencing | Phases · dependency order · milestones | ⬜ Not started |

## 15.1 Dependency Order

```
G0 RECONCILIATION ──┐
G1 SCREEN ANALYSIS ─┤
                    ▼
              D2 PROJECT_OVERVIEW
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      D3 PRD    D4 DOMAIN   D7 INTEGRATIONS
        │           │            │
        └─────┬─────┘            │
              ▼                  │
       D5 BUSINESS_RULES         │
              │                  │
              ▼                  │
       D6 ARCHITECTURE ←─────────┘
              │
              ▼
       D9 API_CONTRACT
              │
              ▼
       D8 FEATURE_BREAKDOWN
              │
              ▼
       D10 DELIVERY_PLAN
```

## 15.2 ID Assignment Responsibility

| ID prefix | Meaning | Assigned by | Status |
|---|---|---|---|
| `R-*` | Confirmed requirement | G0 | ✅ 104 assigned |
| `I-*` | Inferred requirement | G0 | ✅ 12 assigned |
| `M-*` | Missing decision | G0 | ✅ 14 assigned |
| `X-*` | Conflict | G0 | ✅ 25 assigned |
| `W-*` | Workflow | G0 | ✅ 16 assigned |
| `BA-*` | Business area | **D2** | ✅ 14 assigned |
| `C-*` | Product constraint | **D2** | ✅ 58 assigned |
| `B-*` | Gate 1 blocker | G1 | ✅ 3 assigned |
| `UI-*` | Screen / UI element | **D3 (PRD)** | ⬜ Not yet assigned — screens currently traceable by name and G1 section |
| `ENT-*` | Business entity | **D4 (Domain)** | ⬜ Not yet assigned — entities currently traceable by name and G0 §10 group |
| `BR-*` | Business rule | **D5** | ⬜ Not yet assigned |

---

# 16. Source of Truth & Traceability Rule

> **The project documentation is an interconnected Source of Truth.**
>
> Each document owns a specific category of information, but all documents remain connected through stable identifiers and explicit references.
>
> **No important requirement, decision, rule, workflow, entity, screen, constraint, conflict, or assumption may disappear simply because it is owned by another document.**

## 16.1 Operating Rules

| # | Rule |
|:---:|---|
| 1 | Information may **move** to the document responsible for it. It must never be **lost** |
| 2 | When information is summarised, the identifier and the owning document must both be stated |
| 3 | Existing identifiers are never renumbered |
| 4 | Inferred information remains labelled `INFERRED — NOT CONFIRMED` until explicitly validated |
| 5 | Pending decisions remain labelled `⚠️ PENDING SIGN-OFF` or `⚠️ NEEDS CONFIRMATION` until resolved |
| 6 | Conflicts and their resolutions remain traceable, including superseded ones |
| 7 | Anything without a current or future owner is marked `⚠️ UNMAPPED` |
| 8 | The authority hierarchy in §1.3 governs every future conflict |

## 16.2 Change Protocol

When a pending decision is resolved:

1. Record the resolution against its existing ID (`M-01`, `B2`, etc.)
2. Update this document's §13
3. Update the owning document
4. Do not delete the pending entry — mark it resolved with the date and source

---

# 17. Coverage & Traceability Matrix

| Information type | Identified | Covered here | Detailed owner | Traceable IDs | Missing / Unmapped |
|---|:---:|:---:|---|---|---|
| **Requirements** | 104 | ✅ §3, §4, §11, §12 | `PRD.md` (D3) | `R-*` (104) | None |
| **Confirmed decisions** | 104 (as `R-*`) | ✅ §12 | `PRD.md`, `BUSINESS_RULES.md` | `R-*` | No separate `DEC-*` scheme exists — see §12 note |
| **Business rules** | Not yet enumerated | ⚠️ Referenced | `BUSINESS_RULES.md` (D5) | `BR-*` to be assigned | Rules derive from `R-*` and `W-*`; `BR-*` numbering pending D5 |
| **Roles** | 3 | ✅ §4 | `BUSINESS_RULES.md` (permissions) | §4.1–§4.3 | 6 permission questions marked ⚠️ |
| **Workflows** | 16 | ✅ §7 | `PRD.md`, `BUSINESS_RULES.md` | `W-01` → `W-16` | 15 undefined behaviours listed §7.1 |
| **Entities** | 45 | ✅ §8 | `DOMAIN_FEATURE_MAPPING.md` (D4) | By name, G0 §10.1–§10.10 | `ENT-*` IDs pending D4 |
| **Capabilities** | 116 | ✅ §6 | `FEATURE_BREAKDOWN.md` (D8) | Numbered 1–116 (G0 §8) | None |
| **Business areas** | 14 | ✅ §5 | `DOMAIN_FEATURE_MAPPING.md` (D4) | `BA-01` → `BA-14` | Preliminary — boundaries may shift in D4 |
| **Screens** | 13 new · 10 modified groups · 16 removed | ✅ §9 | `SCREEN_UI_CHANGE_ANALYSIS.md` (G1), then `PRD.md` (D3) | By name, G1 §1–§4 | `UI-*` IDs pending D3 |
| **UI changes** | 14 added · 14 removed | ✅ §9.3, §9.5 | G1, then `PRD.md` | By name, G1 §2, §4 | None |
| **Integrations** | 12 categories | ✅ §10 | `INTEGRATIONS.md` (D7) | `M-08`, `R-DT-01` → `R-DT-03` | Provider selection pending |
| **Constraints** | 58 | ✅ §11 | `PROJECT_OVERVIEW.md` (this document) | `C-01` → `C-58` | None |
| **Pending decisions** | 14 (`M-*`) + 3 (`B-*`) + 7 minor | ✅ §13 | This document, then owning documents | `M-01` → `M-14`, `B1` → `B3` | None |
| **Inferred requirements** | 12 | ✅ §14.2 | G0 §3 | `I-01` → `I-12` | All remain unvalidated |
| **Conflicts / resolutions** | 25 | ✅ §18 | G0 §4 | `X-01` → `X-25` | `X-25` superseded, documented |

## 17.1 Unmapped Items

| Item | Reason | Resolution |
|---|---|---|
| ⚠️ **UNMAPPED — `BR-*` business rule identifiers** | No business rules have been formally numbered yet. Rules currently exist implicitly inside `R-*` requirements and `W-*` workflow descriptions | Will be extracted and numbered in `BUSINESS_RULES.md` (D5). Source `R-*` and `W-*` IDs will be cited on each rule |
| ⚠️ **UNMAPPED — `ENT-*` entity identifiers** | Gate 0 catalogued entities by name only | Will be assigned in `DOMAIN_FEATURE_MAPPING.md` (D4) |
| ⚠️ **UNMAPPED — `UI-*` screen identifiers** | Gate 1 catalogued screens by name only | Will be assigned in `PRD.md` (D3) |

**No information is unmapped. Three identifier schemes are pending assignment; the underlying information is fully catalogued and traceable by name.**

---

# 18. Important Conflicts & Resolutions

Twenty-five conflicts were identified during reconciliation. **Twenty-four are resolved.** Full analysis is owned by **Gate 0 §4**.

## 18.1 Resolution Summary

| Conflict area | IDs | Resolved by |
|---|---|---|
| Scale target | `X-01` | Client: no fixed target |
| Role count | `X-02` | Product owner: three roles |
| Navigation variants | `X-03` | Menu customised to built features |
| Internal assets | `X-04` | External only; Figma sample erroneous |
| Customer visibility in UI text | `X-05` | Wording error; no customer access |
| Cross-customer scope | `X-06` | Not supported |
| Undesigned modules in navigation | `X-07` | Removed |
| Finding ID format | `X-08` | `F-XXXX` adopted |
| Inconsistent dashboard figures | `X-09` | Mock data; logic governs |
| Figma authority vs removals | `X-10` | Removed items deleted from Figma too |
| Manual-only vs automation | `X-11` | "Manual" governs scanning; scheduled jobs permitted |
| No logs vs Timeline | `X-12` | Timeline Events only |
| Provisioning complexity | `X-13` | Instant activation |
| Signal-to-finding ratio | `X-14` | Many signals may support one finding |
| Severity source after rules removal | `X-15` | Fixed mapping table in code |
| SLA fields | `X-16` | Removed |
| Scheduled reports | `X-17` | Removed |
| Remediation approval | `X-18` | Removed |
| Automated validation | `X-19` | Removed; manual only |
| Severity ordering | `X-20` | Four levels as shown in Figma |
| Report type count | `X-21` | Two types aggregating all content |
| VIP references | `X-22` | Out of scope |
| Investigate breadth | `X-23` | Four sections only |
| Provider names | `X-24` | Placeholders; selection pending (`M-08`) |

## 18.2 Superseded Conflict

### `X-25` — Client CF-2 response ambiguity

| Field | Detail |
|---|---|
| **Source A** | Client answer to CF-2: *"هما الاتنين واحد"* ("the two are one") |
| **Source B** | Product owner decision: three roles |
| **Nature** | The client response cannot be interpreted with confidence. It may mean the six PRD personas and the two Figma roles describe the same people, or that Internal Admin and Security Lead are one role |
| **Resolution** | Superseded by the later, explicit product-owner decision establishing three roles (`R-ID-01`) |
| **Client confirmation required** | No |
| **Why retained** | Recorded for traceability. If role structure is ever questioned, this ambiguity is the origin |

## 18.3 Conflicts Introduced by Resolutions

Two decisions created downstream tension that is documented rather than hidden:

| Tension | Detail | Status |
|---|---|---|
| **Manual scanning vs recurring jobs** | `R-SN-02` states manual only. `R-TD-06` (three-day verification) and `R-CU-09` (expiry alerting) require recurring jobs | Resolved via `X-11` and `I-01`: "manual" constrains scanning; scheduled jobs are permitted for verification, expiry, and limit checks |
| **Rules removal vs severity requirement** | `R-FD-16` removes the rules interface. Severity is required by scoring | Resolved via `X-15`: severity comes from a fixed table (`R-FD-14`), with the table contents pending (`M-03`) |

---

# 19. Documentation Consistency Verification

Performed against Gate 0, Gate 1, client requirements, and client decisions.

| # | Check | Result | Evidence |
|:---:|---|:---:|---|
| 1 | Every confirmed requirement is represented or owned | ✅ | 104 `R-*` IDs appear across §3, §4, §11, §12 |
| 2 | All confirmed roles are represented | ✅ | 3 roles in §4 with boundaries and isolation rules |
| 3 | All in-scope decisions represented | ✅ | 30 areas in §3.2 |
| 4 | All out-of-scope decisions represented | ✅ | 30 exclusions in §3.3 |
| 5 | All major workflows represented | ✅ | 16 in §7, plus 15 undefined behaviours in §7.1 |
| 6 | All major entities accounted for | ✅ | 45 in §8 across 10 groups |
| 7 | Screens and UI changes accounted for | ✅ | §9: 13 new, 14 added, 16 removed screens, 14 removed elements |
| 8 | Confirmed and pending decisions separated | ✅ | §12 confirmed · §13 pending · §14 classification |
| 9 | Inferred information labelled | ✅ | 12 `I-*` items in §14.2, never presented as fact |
| 10 | Conflicts and resolutions traceable | ✅ | §18: 24 resolved, 1 superseded and retained |
| 11 | Existing IDs preserved | ✅ | No `R-*`, `I-*`, `M-*`, `X-*`, `W-*` renumbered |
| 12 | Every category has an owner | ✅ | §15 documentation map · §17 coverage matrix |
| 13 | Unmapped information marked | ✅ | 3 identifier schemes marked in §17.1 with assignment plans |
| 14 | Capabilities preserved | ✅ | All 116 mapped in §6 |
| 15 | Constraints captured | ✅ | 58 in §11 |
| 16 | No invented requirements | ✅ | Every statement traced to `R-*`, G0, G1, or marked INFERRED / PENDING |
| 17 | Billing not invented | ✅ | Explicitly excluded in §2.6 and `C-58`; contract limits are monitoring caps, not billed quantities |
| 18 | No implementation detail introduced | ✅ | No APIs, schemas, libraries, or code structures |

---

**End of PROJECT_OVERVIEW.md**
