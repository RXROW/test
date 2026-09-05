# SCREEN & UI CHANGE ANALYSIS

**Gate:** 1
**Date:** August 2026
**Audience:** Frontend team
**Basis:** Approved Requirements Reconciliation (Gate 0) · Client answers · Current Figma

---

## Purpose

This document tells the frontend team three things:

1. **Which screens must be created** that do not exist in Figma today
2. **What data each screen needs** to display
3. **What must NOT be built**, because it was removed by an approved decision even though it still appears in Figma

> ⚠️ **The current Figma is larger than the approved scope.** Several designed screens were removed by explicit decision. Section 3 and Section 4 exist specifically to prevent them being built by mistake.

**Reference key:** `R-XX-NN` refers to a confirmed requirement in the Gate 0 reconciliation.

---

# 1. NEW / REQUIRED SCREENS

Eight screens are required by approved requirements but do not exist in Figma.

---

## 1.1 Login

### Screen
| Field | Detail |
|---|---|
| **Name** | Login |
| **Roles** | Super Admin · Admin · Security Analyst |
| **Domain** | Identity & Access |
| **Features** | Authentication · account lockout |
| **Purpose** | Single entry point for all three roles |
| **Source** | R-ID-17 · R-ID-20 · R-ID-07 |

### Required UI Data

**FORM DATA**
- Email
- Password

**DISPLAY DATA**
- Error message (invalid credentials)
- Lockout message with remaining wait time
- Product name and logo
- Language toggle (EN / AR)

**ACTION DATA**
- Sign in
- Forgot password

> **Note:** All three roles use this screen. Routing after login is determined by role — Super Admin lands on Admin Management, Admin on Customers, Analyst on Workspace Dashboard.

---

## 1.2 Set Password (Invitation Acceptance)

### Screen
| Field | Detail |
|---|---|
| **Name** | Set Password |
| **Roles** | Admin · Security Analyst (invitees) |
| **Domain** | Identity & Access |
| **Features** | Email invitation flow |
| **Purpose** | Allow an invited user to set their password and activate their account |
| **Source** | R-ID-09 · W-02 · W-03 |

### Required UI Data

**DISPLAY DATA**
- Invited email address (read-only)
- Inviter name
- Role being granted
- Invitation expiry notice
- Password requirements

**FORM DATA**
- New password
- Confirm password

**ACTION DATA**
- Set password and continue

**ERROR STATES**
- Invitation expired
- Invitation already used

---

## 1.3 Forgot / Reset Password

### Screen
| Field | Detail |
|---|---|
| **Name** | Forgot Password · Reset Password |
| **Roles** | Admin · Security Analyst |
| **Domain** | Identity & Access |
| **Features** | Password recovery |
| **Purpose** | Recover access without administrator involvement |
| **Source** | Implied by R-ID-09 (email-based accounts) |

> ⚠️ **NEEDS CONFIRMATION** — Password recovery was not explicitly confirmed. Super Admin recovery is by script (R-ID-08), but no decision was recorded for Admin and Analyst.

### Required UI Data

**FORM DATA (request step)**
- Email

**FORM DATA (reset step)**
- New password
- Confirm password

**DISPLAY DATA**
- Confirmation that an email was sent
- Link expiry notice

---

## 1.4 Super Admin — Admin Management

### Screen
| Field | Detail |
|---|---|
| **Name** | Admin Management |
| **Roles** | Super Admin only |
| **Domain** | Identity & Access |
| **Features** | Create Admin · delete Admin with reassignment |
| **Purpose** | The only screen available to Super Admin |
| **Source** | R-ID-03 · R-ID-04 · R-ID-06 · R-ID-22 |

### Required UI Data

**TABLE DATA** — one row per Admin
- Admin name
- Admin email
- Status (Invited · Active)
- Date created
- Number of customers owned
- Actions (Delete)

**FORM DATA** — invite Admin
- Email address

**ACTION DATA**
- Invite Admin
- Delete Admin
- Resend invitation

**FORM DATA** — delete Admin (required step)
- Replacement Admin (dropdown of other Admins)
- Confirmation

**DISPLAY DATA** — deletion dialog
- Number of customers that will transfer
- List of affected customer names
- Warning that deletion is blocked without a replacement

> **Critical:** Deletion **cannot proceed** without selecting a replacement (R-ID-23). The dropdown is mandatory.

---

## 1.5 Analyst Management

### Screen
| Field | Detail |
|---|---|
| **Name** | Analyst Management |
| **Roles** | Admin |
| **Domain** | Identity & Access |
| **Features** | Create Analyst · delete Analyst with reassignment |
| **Purpose** | Manage the shared pool of Security Analysts |
| **Source** | R-ID-09 · R-ID-11 · R-ID-21 |

### Required UI Data

**TABLE DATA** — one row per Analyst
- Analyst name
- Analyst email
- Status (Invited · Active)
- Date created
- Number of customers assigned
- Actions (Delete)

**FILTER DATA**
- Status
- Search by name or email

**FORM DATA** — invite Analyst
- Email address

**ACTION DATA**
- Invite Analyst
- Delete Analyst
- Resend invitation

**FORM DATA** — delete Analyst (required step)
- Replacement Analyst (dropdown)
- Confirmation

**DISPLAY DATA** — deletion dialog
- Number of customers that will transfer
- List of affected customer names

> **Important:** All Admins see **all** Analysts, not only ones they created (R-ID-11). But the customer counts shown should reflect only customers owned by the viewing Admin.
>
> ⚠️ **NEEDS CONFIRMATION** — whether the "customers assigned" count is global or scoped to the viewing Admin.

---

## 1.6 Monitoring Scope Configuration

### Screen
| Field | Detail |
|---|---|
| **Name** | Monitoring Scope |
| **Roles** | Admin |
| **Domain** | Customer Management |
| **Features** | Define what the system monitors for a customer |
| **Purpose** | Supply the inputs that Brand Protection, Threat Intelligence, and Dark Web modules require |
| **Source** | R-CU-13 · R-CU-14 |

> This is the **most important new screen**. Without it, four modules have no input data.

### Required UI Data

**DISPLAY DATA**
- Customer name
- Customer ID
- Enabled modules (to indicate which sections are relevant)

**FORM DATA — Domains & Infrastructure**
- Root domains (repeatable list; the primary domain appears first and is read-only)
- IP ranges / CIDR blocks (repeatable, optional)
- Cloud account identifiers (repeatable, optional)

**FORM DATA — Brand**
- Brand keywords (repeatable; each with a language marker EN / AR)
- Official social media handles (repeatable; each with platform + handle)

**FORM DATA — Identity**
- Corporate email domains (repeatable)

**FORM DATA — Exclusions**
- Excluded domains / IPs / keywords (repeatable, with an optional reason)

**ACTION DATA**
- Add entry (per list)
- Remove entry
- Save

**VALIDATION DISPLAY**
- Duplicate entry warning
- Format validation per field type

> ⚠️ **NEEDS CONFIRMATION (M-10)** — the field list above is proposed, not signed off. Each field maps to a specific module requirement, so removing a field disables the related capability.

| Field group | Enables |
|---|---|
| Root domains · IP ranges · Cloud accounts | Asset discovery |
| Brand keywords | Phishing domain detection |
| Social handles | Impersonation detection |
| Corporate email domains | Leaked credential detection |
| Exclusions | False-positive reduction across all modules |

---

## 1.7 Pending Review Queue

### Screen
| Field | Detail |
|---|---|
| **Name** | Pending Review |
| **Roles** | Security Analyst |
| **Domain** | Findings |
| **Features** | Approve or reject findings before they enter the active queue |
| **Purpose** | Gate that prevents unreviewed findings reaching the main workflow |
| **Source** | R-FD-03 · R-FD-04 · R-FD-05 · R-FD-06 |

### Required UI Data

**SUMMARY / METRIC DATA**
- Total pending count
- Count pending more than 7 days (these also appear in the main queue — R-FD-07)
- Count by severity

**TABLE DATA** — one row per pending finding
- Selection checkbox
- Finding title
- Affected asset
- Severity
- Confidence
- Source module
- Number of supporting signals
- Detected date
- Days pending
- Actions (Approve · Reject · View detail)

**FILTER DATA**
- Severity
- Source module
- Days pending
- Asset

**SEARCH DATA**
- Finding title or asset

**ACTION DATA**
- Approve selected (bulk)
- Reject selected (bulk)
- Approve single
- Reject single
- Select all

**DISPLAY DATA — rejection**
- Confirmation that rejection marks the finding as False Positive, not deleted

> **Note:** Approval and rejection must work both individually and in bulk (R-FD-04).

---

## 1.8 Takedown Management

### Screen
| Field | Detail |
|---|---|
| **Name** | Takedown Queue · Takedown Detail |
| **Roles** | Security Analyst |
| **Domain** | Takedown |
| **Features** | Prepare · review · send · track · verify |
| **Purpose** | Manage removal requests for malicious content |
| **Source** | R-TD-01 → R-TD-09 |

> This is **two screens**: a list and a detail view.

### Required UI Data — Takedown Queue

**SUMMARY / METRIC DATA**
- Draft count
- Ready to send count
- Sent count (awaiting response)
- Completed this quarter
- Quota remaining this quarter

**TABLE DATA** — one row per request
- Target value (domain, app name, account handle)
- Target type
- Recipient (registrar / host / platform)
- Status
- Date prepared
- Date sent
- Days since sent
- Actions (Open)

**FILTER DATA**
- Status
- Target type
- Date range

**ACTION DATA**
- Open request

### Required UI Data — Takedown Detail

**DISPLAY DATA**
- Target value and type
- Linked signal / finding
- Current status
- Prepared date · sent date · completed date
- Quota consumption notice

**DISPLAY DATA — evidence**
- Screenshot (image)
- Ownership record (WHOIS text)
- Similarity comparison result
- Capture timestamps

**DISPLAY DATA — recipient**
- Recipient name
- Abuse email address
- Abuse form URL (if applicable)

**FORM DATA — request body**
- Draft text (editable before sending)

**FORM DATA — record response**
- Response received (yes/no)
- Response text
- Outcome (Acknowledged · Rejected · Completed)

**ACTION DATA**
- Copy request text
- Copy recipient address
- Mark as sent
- Record response
- Verify removal (manual trigger)
- Close request

**DISPLAY DATA — verification**
- Last verification date
- Verification result (still live / removed)
- Next automatic check date

> **Note:** The system does **not** send the request. It prepares the text and the analyst sends it externally (R-TD-01). The screen must make this explicit — the primary action is "Copy" and "Mark as sent", not "Send".

---

## 1.9 Public Report View

### Screen
| Field | Detail |
|---|---|
| **Name** | Public Report View |
| **Roles** | **No login** — external recipient |
| **Domain** | Reporting |
| **Features** | Password-protected report access |
| **Purpose** | The only screen a customer ever sees |
| **Source** | R-EX-02 · R-EX-08 · R-EX-09 |

> ⚠️ This screen is **outside the authenticated application**. It must not include navigation, user menus, or any link back into the product.

### Required UI Data — Password Gate

**FORM DATA**
- Password

**DISPLAY DATA**
- Customer name
- Report title
- Report date
- Error message (wrong password)
- Expiry message (link expired)
- Revocation message (link revoked)

### Required UI Data — Report Content

**DISPLAY DATA — header**
- Customer name
- Report type (Executive / Technical)
- Generation date
- Reporting period

**SUMMARY / METRIC DATA**
- Total findings
- Findings by severity
- Overall risk score
- Assets monitored

**DISPLAY DATA — narrative**
- Executive summary text
- Key observations (template-generated)

**TABLE DATA — findings**
- Finding title
- Affected asset
- Severity
- Risk score
- Status
- Recommended action

**ACTION DATA**
- Download as PDF

> **Note:** The link opens a web page, not a PDF file. The PDF is generated on demand from that page (R-EX-08, R-EX-09).

---

## 1.10 Profile & Settings

### Screen
| Field | Detail |
|---|---|
| **Name** | Profile · Settings |
| **Roles** | All authenticated roles |
| **Domain** | Identity & Access |
| **Features** | Personal details · session management · preferences |
| **Purpose** | User self-service |
| **Source** | R-CU-01 |

> ⚠️ **NEEDS CONFIRMATION (M-13)** — these screens are confirmed in scope but have no design and no defined content. The list below is the minimum implied by other requirements.

### Required UI Data — Profile

**DISPLAY DATA**
- Name
- Email
- Role
- Date joined

**FORM DATA**
- Name (editable)
- Change password (current · new · confirm)

**TABLE DATA — sessions**
- Device / browser
- IP address
- Last active
- Current session indicator
- Action (End session)

**ACTION DATA**
- Save changes
- End session
- End all other sessions

### Required UI Data — Settings

**FORM DATA**
- Language preference (EN / AR)
- Email notification toggle
- In-app notification toggle

**FORM DATA — Admin only**
- Default secure link validity period

> **Note:** Link validity is set by the Admin (R-EX-05). Whether this belongs in Settings or on the report-sharing dialog is a design decision.

---

## 1.11 Notification Panel

### Screen
| Field | Detail |
|---|---|
| **Name** | Notification Panel |
| **Roles** | Admin · Security Analyst |
| **Domain** | Notifications |
| **Features** | In-app notifications |
| **Purpose** | Surface the four alert types inside the application |
| **Source** | R-NT-01 · R-NT-02 |

> ⚠️ **No notification icon exists anywhere in the current Figma.** This is a genuine gap — in-app notifications were confirmed but never designed.

### Required UI Data

**DISPLAY DATA — bell indicator**
- Unread count

**TABLE DATA — notification list**
- Notification type
- Message
- Related entity (customer / finding / scan)
- Timestamp
- Read / unread state
- Action link

**FILTER DATA**
- Unread only

**ACTION DATA**
- Mark as read
- Mark all as read
- Navigate to related item

**Notification types (four confirmed):**

| Type | Recipient | Related to |
|---|---|---|
| New critical finding | Assigned Analyst | Finding |
| Scan failed | Initiating Analyst | Scan run |
| Customer approaching limit | Owning Admin | Customer |
| Contract expiring soon | Owning Admin | Customer |

> ⚠️ **NEEDS CONFIRMATION (M-14)** — recipient mapping is inferred, not confirmed.

---

# 2. ADDED FEATURES / UI ELEMENTS

Changes to screens that already exist in Figma.

---

## 2.1 Customer Form — Monitoring Scope Section

| Field | Detail |
|---|---|
| **Element** | New section or link to a separate screen |
| **Screen** | Tenant Configuration (Customer Create/Edit) |
| **Description** | Access point to the Monitoring Scope configuration |
| **Source** | R-CU-13 |
| **Required data** | Completion indicator (configured / not configured) · link or expand action |

---

## 2.2 Customer Form — Analyst Assignment

| Field | Detail |
|---|---|
| **Element** | New required field |
| **Screen** | Tenant Configuration |
| **Description** | Assign exactly one Analyst; activation is blocked without it |
| **Source** | R-ID-14 · R-CU-08 |
| **Required data** | Analyst dropdown (all Analysts) · currently assigned Analyst · validation message when empty |

> **Critical:** The "Save & Activate Monitoring" button must be disabled until an Analyst is selected.

---

## 2.3 Customer List — Assigned Analyst Column

| Field | Detail |
|---|---|
| **Element** | New table column |
| **Screen** | Customer Directory |
| **Description** | Show which Analyst is responsible for each customer |
| **Source** | R-ID-14 |
| **Required data** | Analyst name |

---

## 2.4 Customer List — Analyst Filter

| Field | Detail |
|---|---|
| **Element** | New filter |
| **Screen** | Customer Directory |
| **Description** | Filter customers by assigned Analyst |
| **Source** | R-ID-12 |
| **Required data** | Analyst list |

---

## 2.5 Customer Detail — Permanent Delete Action

| Field | Detail |
|---|---|
| **Element** | New action, separate from the existing Delete |
| **Screen** | Customer Detail |
| **Description** | Irreversible removal, distinct from soft delete |
| **Source** | R-CU-02 · R-CU-03 |
| **Required data** | Confirmation dialog · warning text · typed confirmation |

> The existing Delete button becomes **soft delete**. Permanent delete is a second, clearly separated action available only to Admins.

---

## 2.6 Customer Detail — Contract Extension

| Field | Detail |
|---|---|
| **Element** | New action |
| **Screen** | Customer Detail |
| **Description** | Extend contract by changing the end date, restoring Active status |
| **Source** | R-CU-12 |
| **Required data** | New end date · current status · confirmation |

---

## 2.7 Asset Inventory — Pending Approval State

| Field | Detail |
|---|---|
| **Element** | New status value and filter |
| **Screen** | Asset Inventory |
| **Description** | Assets awaiting Analyst approval before monitoring begins |
| **Source** | R-AS-04 |
| **Required data** | Pending count · status badge · approve/reject actions · bulk selection |

---

## 2.8 Asset Inventory — Deleted Indicator

| Field | Detail |
|---|---|
| **Element** | New status indicator and filter |
| **Screen** | Asset Inventory · Asset Detail |
| **Description** | Mark assets that no longer respond, without removing them |
| **Source** | R-AS-05 |
| **Required data** | Deleted flag · date last seen · filter (show/hide deleted) |

---

## 2.9 Limit Overrun — Pending Approval

| Field | Detail |
|---|---|
| **Element** | New state and action |
| **Screen** | Asset Inventory · Customer Detail |
| **Description** | Items beyond a contract limit await Analyst approval |
| **Source** | R-CU-05 · R-CU-06 |
| **Required data** | Limit reached banner · pending item count · approve/reject actions · current usage vs limit |

---

## 2.10 Findings — False Positive Status

| Field | Detail |
|---|---|
| **Element** | New status value and filter |
| **Screen** | Findings List |
| **Description** | Rejected findings are retained and marked, not deleted |
| **Source** | R-FD-06 |
| **Required data** | Status badge · filter option |

---

## 2.11 Findings — Recurrence Indicator

| Field | Detail |
|---|---|
| **Element** | New column and detail field |
| **Screen** | Findings List · Finding Detail |
| **Description** | Show how many times a finding has recurred |
| **Source** | R-FD-11 |
| **Required data** | Recurrence count · first detected date · last reopened date |

---

## 2.12 Finding Detail — Supporting Signals

| Field | Detail |
|---|---|
| **Element** | Existing section, now confirmed as many-to-one |
| **Screen** | Finding Detail |
| **Description** | Multiple signals support one finding |
| **Source** | R-FD-01 · R-FD-12 |
| **Required data** | Signal list (type · source · confidence · date) · aggregated confidence · signal count |

---

## 2.13 Report Sharing Dialog

| Field | Detail |
|---|---|
| **Element** | New dialog |
| **Screen** | Report screens |
| **Description** | Generate a password-protected link and email it to the customer contact |
| **Source** | R-EX-02 → R-EX-06 |
| **Required data** | Recipient email (from customer record, read-only) · validity period · generated link · generated password · copy actions · revoke action |

---

## 2.14 Language Toggle

| Field | Detail |
|---|---|
| **Element** | Existing in Figma, confirmed in scope |
| **Screen** | All authenticated screens |
| **Description** | EN / AR switch with RTL layout |
| **Source** | S4 |
| **Required data** | Current language · available languages |

---

# 3. REMOVED SCREENS

> ⚠️ **These screens exist in Figma but must NOT be built.**

---

## 3.1 Detection Rules / Signals View

| Field | Detail |
|---|---|
| **Previous purpose** | View, edit, preview and compare detection rules |
| **Evidence** | R-FD-16 — no user-editable rules interface |
| **Replaced by** | Nothing. Severity now comes from a fixed mapping table maintained in code (R-FD-14) |
| **Impact** | The Detect & Score tab bar loses the "Rules" tab |

---

## 3.2 Investigate — Actor View

| Field | Detail |
|---|---|
| **Previous purpose** | Threat actor profiles with TTPs, campaigns, infrastructure |
| **Evidence** | R-CO-05 — Investigate includes Overview, Graph, Entity, Timeline only |
| **Replaced by** | Actor information appears as nodes in the Graph and as attributes on Entity Detail |
| **Impact** | Tab removed |

---

## 3.3 Investigate — Campaign View

| Field | Detail |
|---|---|
| **Previous purpose** | Campaign tracking with checkpoints, linked actors, victims |
| **Evidence** | R-CO-05 |
| **Replaced by** | Campaign appears as a node type in the Graph |
| **Impact** | Tab removed |

---

## 3.4 Investigate — IOC / Signal Mapping View

| Field | Detail |
|---|---|
| **Previous purpose** | IOC inventory with per-source verdicts and infrastructure mapping |
| **Evidence** | R-CO-05 |
| **Replaced by** | IOCs appear as nodes in the Graph; source verdicts appear on Entity Detail |
| **Impact** | Tab removed |

---

## 3.5 Investigate — Correlation View

| Field | Detail |
|---|---|
| **Previous purpose** | Correlated finding clusters with shared root cause |
| **Evidence** | R-CO-05 |
| **Replaced by** | Clustering still runs (R-CO-01) but is surfaced through the Clusters tab in Detect & Score |
| **Impact** | Tab removed from Investigate |

> ⚠️ **NEEDS CONFIRMATION** — clustering is confirmed in scope (R-CO-01) and the Clusters tab exists in Detect & Score. Confirm that removing the Investigate Correlation tab does not also remove the Detect & Score Clusters tab.

---

## 3.6 Investigate — Workspace View

| Field | Detail |
|---|---|
| **Previous purpose** | Saved investigation state with pinned entities and notes |
| **Evidence** | R-CO-05 · R-CO-06 (no sharing) |
| **Replaced by** | Nothing |
| **Impact** | Tab removed |

---

## 3.7 Saved Investigation Views

| Field | Detail |
|---|---|
| **Previous purpose** | Reusable saved graph states, shared across analysts |
| **Evidence** | R-CO-06 — investigations are not shared |
| **Replaced by** | Nothing |
| **Impact** | Screen removed |

---

## 3.8 Investigation Templates / Quick Start

| Field | Detail |
|---|---|
| **Previous purpose** | Pre-configured investigation starting points |
| **Evidence** | R-CO-05 — not among the four retained sections |
| **Replaced by** | Nothing |
| **Impact** | Screen removed |

---

## 3.9 Monitor Module

| Field | Detail |
|---|---|
| **Previous purpose** | Change tracking dashboard (navigation item only, never designed) |
| **Evidence** | Gate 0 §6.2 — module removed |
| **Replaced by** | Recurrence tracking continues internally and surfaces through the recurrence counter on findings (R-FD-11) |
| **Impact** | Navigation item removed |

---

## 3.10 Predict Module

| Field | Detail |
|---|---|
| **Previous purpose** | Forecasting (navigation item only, never designed) |
| **Evidence** | Gate 0 §6.2 — module removed |
| **Replaced by** | Risk Reduction Preview remains inside the Remediation screen as a calculation (R-RM-06) |
| **Impact** | Navigation item removed |

---

## 3.11 VIP Protection Module

| Field | Detail |
|---|---|
| **Previous purpose** | Executive impersonation monitoring |
| **Evidence** | R-SC-03 · Gate 0 §6.2 |
| **Replaced by** | Nothing |
| **Impact** | Module removed from the customer entitlement list; the five module toggles become four |

---

## 3.12 Admin Console — Dashboard · Sales · Partners · Demo Requests · Modules · Schedule

| Field | Detail |
|---|---|
| **Previous purpose** | Business operations (navigation items only, never designed) |
| **Evidence** | R-CU-01 — Admin Console contains Customers, Profile, Settings only |
| **Replaced by** | Nothing |
| **Impact** | Six navigation items removed from the Admin sidebar |

---

## 3.13 Scan Module

| Field | Detail |
|---|---|
| **Status** | ⚠️ **NEEDS CONFIRMATION** |
| **Previous purpose** | Scan Overview · Service & Ports · Vulnerabilities |
| **Evidence** | R-SN-01 confirms Scan and Assessment are two separate stages, which implies Scan is retained. However the three Scan screens duplicate Assessment screens almost exactly |
| **Question** | Are the three Scan screens built, or is Scan a backend stage with no dedicated screens? |
| **Impact if removed** | Three screens removed; Assessment becomes the only analyst-facing view |

---

# 4. REMOVED UI FEATURES / ELEMENTS

> ⚠️ **These elements exist in Figma but must NOT be built.**

---

## 4.1 SLA Fields and Due Dates

| Field | Detail |
|---|---|
| **Screen** | Prioritization Queue · Remediation Guidance · Findings List |
| **Previously did** | Displayed target SLA, due dates, countdown timers, breach counts, SLA likelihood |
| **Evidence** | R-RS-05 · R-RS-06 — no time-based targets |
| **Status** | **Completely removed** |
| **Affected elements** | `Target SLA: 4 days` · `Due 2026-05-17` · `SLA: 8h` · `Over SLA: 6` · `SLA likelihood: 98%` · `SLA Pressure` table |

---

## 4.2 Remediation Approval Drawer

| Field | Detail |
|---|---|
| **Screen** | Assessment Finding Intelligence |
| **Previously did** | Compare current exposure with proposed fix; Approve / Request Edits |
| **Evidence** | R-RM-04 — no approval workflow |
| **Status** | **Completely removed** |

---

## 4.3 Automated Validation

| Field | Detail |
|---|---|
| **Screen** | Assessment Finding Intelligence · Remediation Guidance |
| **Previously did** | "Run Verification" triggered an automated re-check with a progress screen |
| **Evidence** | R-RM-03 — verification is manual |
| **Status** | **Removed as automation.** The Validation Checklist remains as a manual checklist the Analyst ticks |
| **Affected elements** | `Run Verification` button · `Validation is running` progress screen · `Validate Access` button |

> **Clarification:** The checklist items stay. The automatic execution does not.

---

## 4.4 Scheduled Reports

| Field | Detail |
|---|---|
| **Screen** | Export / Reporting Entry |
| **Previously did** | Recurring report generation and delivery |
| **Evidence** | R-RP-05 |
| **Status** | **Completely removed** |
| **Affected elements** | `Scheduled Reports: 6` metric card |

---

## 4.5 Cross-Customer Scope

| Field | Detail |
|---|---|
| **Screen** | Findings List · Alerts Center |
| **Previously did** | Saved views and alert rules with `Scope: All tenants` |
| **Evidence** | R-ID-16 — no cross-customer visibility |
| **Status** | **Completely removed** |
| **Affected elements** | `Scope: All tenants` column value |

---

## 4.6 Saved Views (all screens)

| Field | Detail |
|---|---|
| **Screen** | Findings List · Assessment · Investigate · Vulnerability Results |
| **Previously did** | Save and share filter combinations |
| **Evidence** | R-CO-06 — no sharing |
| **Status** | ⚠️ **NEEDS CONFIRMATION** |
| **Question** | R-CO-06 removes *sharing*. Does it also remove *private* saved views? |
| **Affected elements** | `Saved Views: 6` metric · `Saved Filter Views` table · `Save as View` button · `Apply View` action |

---

## 4.7 Alert Rules

| Field | Detail |
|---|---|
| **Screen** | Alerts / Notifications Center |
| **Previously did** | Configure how alerts are triggered, grouped, and suppressed |
| **Evidence** | R-NT-02 — four fixed alert triggers |
| **Status** | **Removed as configurable.** The four triggers are fixed |
| **Affected elements** | `Alert Rules` table · `Muted: 4` metric · suppression configuration |

---

## 4.8 Automated Takedown Sending

| Field | Detail |
|---|---|
| **Screen** | Fraud Intelligence · Domain Detail |
| **Previously did** | Implied automatic submission to registrars |
| **Evidence** | R-TD-01 — analyst sends manually |
| **Status** | **Removed as automation.** Preparation is automatic; sending is manual |

---

## 4.9 Audit Log

| Field | Detail |
|---|---|
| **Screen** | Not designed, but implied by BRD |
| **Previously did** | Compliance-oriented action history |
| **Evidence** | R-NT-03 — Timeline Events only |
| **Status** | **Completely removed.** Timeline Events remain and are visible in the Investigate Timeline |

---

## 4.10 Three-Stage Provisioning Display

| Field | Detail |
|---|---|
| **Screen** | Customer Detail |
| **Previously did** | Showed Onboarding Status · Activation Status · Discovery Initialization as three separate indicators with a background sequence |
| **Evidence** | R-CU-07 — activation is instantaneous |
| **Status** | **Replaced by a single status indicator** (Active / Not Active / Pending) |
| **Affected elements** | `Workspace / Monitoring Status` card with three rows · `Baseline running` state · `Open Workspace becomes available after the provisioning sequence` message |

---

## 4.11 Continuous Discovery Messaging

| Field | Detail |
|---|---|
| **Screen** | Discover & Map Overview · Assessment Overview |
| **Previously did** | Stated "Continuous discovery" and "running continuously with evidence from the last 6 hours" |
| **Evidence** | R-SN-02 — manual scanning only |
| **Status** | **Text must change.** Replace with last-scan timestamp and a manual scan action |

---

## 4.12 Two-Factor Authentication

| Field | Detail |
|---|---|
| **Screen** | Not designed |
| **Evidence** | R-ID-18 |
| **Status** | **Not implemented** |

---

## 4.13 Single Sign-On

| Field | Detail |
|---|---|
| **Screen** | Not designed |
| **Evidence** | R-ID-19 |
| **Status** | **Not implemented** |

---

## 4.14 Self-Service Signup

| Field | Detail |
|---|---|
| **Screen** | Not designed |
| **Evidence** | R-ID-02 — no signup; accounts are created by invitation |
| **Status** | **Not implemented** |

---

# 5. FRONTEND IMPLEMENTATION REFERENCE

| Screen | Status | Role | Domain | Main Feature | Required Data |
|---|---|---|---|---|---|
| Login | **NEW** | All | Identity | Authentication | Email · password · errors · lockout |
| Set Password | **NEW** | Admin · Analyst | Identity | Invitation acceptance | Email · role · password fields |
| Forgot Password | **NEW** ⚠️ | Admin · Analyst | Identity | Recovery | Email · reset fields |
| Admin Management | **NEW** | Super Admin | Identity | Create/delete Admins | Admin list · invite form · reassignment |
| Analyst Management | **NEW** | Admin | Identity | Create/delete Analysts | Analyst list · invite form · reassignment |
| Monitoring Scope | **NEW** | Admin | Customer | Define monitoring inputs | Domains · brands · handles · email domains · exclusions |
| Pending Review | **NEW** | Analyst | Findings | Approve/reject findings | Pending list · bulk actions · filters |
| Takedown Queue | **NEW** | Analyst | Takedown | Request list | Request list · quota · filters |
| Takedown Detail | **NEW** | Analyst | Takedown | Prepare and track | Evidence · recipient · draft · status |
| Public Report View | **NEW** | External | Reporting | Password-gated report | Password gate · report content · PDF download |
| Profile | **NEW** ⚠️ | All | Identity | Personal details · sessions | Name · email · role · session list |
| Settings | **NEW** ⚠️ | All | Identity | Preferences | Language · notification toggles |
| Notification Panel | **NEW** | Admin · Analyst | Notifications | In-app alerts | Unread count · notification list |
| Customer Directory | **MODIFIED** | Admin | Customer | Customer list | + Analyst column · + Analyst filter |
| Tenant Configuration | **MODIFIED** | Admin | Customer | Create/edit customer | + Analyst assignment · + Monitoring Scope link |
| Customer Detail | **MODIFIED** | Admin | Customer | Customer overview | + permanent delete · + contract extension · − 3-stage provisioning |
| Workspace Dashboard | **MODIFIED** | Analyst | Dashboard | Analyst home | − continuous messaging · − SLA metrics |
| Assessment (9 screens) | **MODIFIED** | Analyst | Assessment | Security assessment | − SLA fields · − validation automation · − approval drawer |
| Discover & Map (9 tabs) | **MODIFIED** | Analyst | Discovery | Asset and signal discovery | + pending approval · + deleted indicator · − continuous messaging |
| Asset Inventory | **MODIFIED** | Analyst | Assets | Asset management | + pending state · + deleted flag · + limit banner |
| Detect & Score | **MODIFIED** | Analyst | Findings | Findings workflow | + false positive · + recurrence · − Rules tab · − SLA · − saved views ⚠️ |
| Investigate | **MODIFIED** | Analyst | Investigation | Graph investigation | Four tabs only: Overview · Graph · Entity · Timeline |
| Report & Prioritize | **MODIFIED** | Analyst | Reporting | Reports | + sharing dialog · − scheduled reports · − SLA |
| Detection Rules | **REMOVED** | — | — | — | — |
| Investigate: Actor | **REMOVED** | — | — | — | — |
| Investigate: Campaign | **REMOVED** | — | — | — | — |
| Investigate: IOC | **REMOVED** | — | — | — | — |
| Investigate: Correlation | **REMOVED** ⚠️ | — | — | — | — |
| Investigate: Workspace | **REMOVED** | — | — | — | — |
| Saved Investigation Views | **REMOVED** | — | — | — | — |
| Investigation Templates | **REMOVED** | — | — | — | — |
| Monitor module | **REMOVED** | — | — | — | — |
| Predict module | **REMOVED** | — | — | — | — |
| VIP Protection | **REMOVED** | — | — | — | — |
| Admin: Dashboard | **REMOVED** | — | — | — | — |
| Admin: Sales | **REMOVED** | — | — | — | — |
| Admin: Partners | **REMOVED** | — | — | — | — |
| Admin: Demo Requests | **REMOVED** | — | — | — | — |
| Admin: Modules | **REMOVED** | — | — | — | — |
| Admin: Schedule | **REMOVED** | — | — | — | — |
| Scan (3 screens) | **NEEDS CONFIRMATION** | Analyst | Scanning | Raw scan results | Duplicates Assessment — confirm retention |

## 5.1 Count Summary

| Status | Count |
|---|:---:|
| NEW | 13 |
| MODIFIED | 10 groups |
| REMOVED | 16 |
| NEEDS CONFIRMATION | 1 group (3 screens) |

---

# 6. FINAL CROSS-CHECK

| # | Check | Result |
|:---:|---|:---:|
| 1 | Every newly required screen is listed | ✅ 13 screens in §1 |
| 2 | Every added UI feature is listed | ✅ 14 items in §2 |
| 3 | Every removed screen is listed | ✅ 16 items in §3 |
| 4 | Every removed UI feature is listed | ✅ 14 items in §4 |
| 5 | Every new screen has required data identified | ✅ All 13 |
| 6 | No removed feature appears as active | ✅ Verified against §3 and §4 |
| 7 | Nothing invented beyond approved requirements | ✅ Every item traced to `R-XX-NN` or marked NEEDS CONFIRMATION |
| 8 | Uncertain items marked | ✅ 6 items marked NEEDS CONFIRMATION |

## 6.1 Items Marked NEEDS CONFIRMATION

| # | Item | Section | Why |
|:---:|---|---|---|
| 1 | Forgot / Reset Password flow | §1.3 | Not explicitly confirmed for Admin and Analyst |
| 2 | Analyst "customers assigned" count scope | §1.5 | Global count or scoped to viewing Admin |
| 3 | Monitoring Scope field list | §1.6 | Proposed, pending sign-off (M-10) |
| 4 | Notification recipient mapping | §1.11 | Inferred (M-14) |
| 5 | Clusters tab retention in Detect & Score | §3.5 | Clustering confirmed in scope but Investigate Correlation removed |
| 6 | Saved Views — private vs shared | §4.6 | R-CO-06 removes sharing; unclear whether private views also go |
| 7 | Scan module screens | §3.13 | Stage confirmed; dedicated screens duplicate Assessment |
| 8 | Profile / Settings content | §1.10 | In scope but undesigned (M-13) |

---

# STATUS

## ⚠️ NOT READY

The screen list is reliable. The **data list is not**, for three screens.

## Blocking Items

Only three items prevent the frontend from working reliably. The rest can proceed.

### B1 — Monitoring Scope field list (M-10)

This is a new screen whose entire content is unconfirmed. The frontend cannot build a form without knowing the fields. Every field maps to a module capability, so an incorrect list silently disables features.

**Needed:** sign-off on the seven field groups in §1.6.

### B2 — Scan module retention

Three screens are either built or not. This is a binary decision affecting the analyst navigation and roughly 5% of the total screen count.

**Needed:** confirm whether Scan has dedicated screens or is a backend-only stage.

### B3 — Saved Views scope

This affects **four existing screens** (Findings List, Assessment, Investigate, Vulnerability Results). Each has a metric card, a table, and two actions that either stay or go.

**Needed:** confirm whether private (unshared) saved views remain.

## Non-Blocking

These can be resolved while frontend work begins:

| # | Item | Why non-blocking |
|:---:|---|---|
| 1 | Forgot password flow | Standard pattern; add or remove one screen |
| 2 | Analyst count scope | Single number on one screen |
| 3 | Notification recipients | Backend concern; frontend renders whatever arrives |
| 4 | Clusters tab | Tab exists in Figma; default to keeping it |
| 5 | Profile / Settings content | Minimal content proposed; low risk |

## To Reach READY

Confirm **B1, B2, and B3**. Nothing else is required.

---

**End of analysis.**
