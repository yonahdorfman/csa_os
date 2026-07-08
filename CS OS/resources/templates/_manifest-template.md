# MANIFEST Template — _MANIFEST.md

**Location:** `/Accounts/[Account Name]/_MANIFEST.md`

**Purpose:** Quick reference guide for this account. Tells you how to read account context (reading priority), lists data sources and collaborators, and tracks who owns what.

**When to update:**
- When adding a new data source (Notion page, Drive folder, Slack channel, Jira project)
- When a co-owner or collaborator joins/leaves
- When source freshness expectations change
- When account tier or reading complexity changes

**Use this file to:**
- Understand what sources of truth exist for this account
- Know who to ask about this account
- Understand the reading strategy (Tier 1 vs. Tier 2 vs. Tier 3)

---

## Header

```
# {ACCOUNT_NAME} — MANIFEST

**Account Tier:** {Tier}  
**Reading Priority:** {Tier 1 | Tier 2 | Tier 3}  
**Created:** YYYY-MM-DD  
**Last Updated:** YYYY-MM-DD  
**Maintained By:** {Your Name}

---
```

Example:
```
# Acme Corp — MANIFEST

**Account Tier:** Enterprise  
**Reading Priority:** Tier 1 (Strategic, requires latest context)  
**Created:** 2024-06-15  
**Last Updated:** 2026-04-10  
**Maintained By:** {CSM_NAME}

---
```

---

## Reading Strategy

Explains how to approach reading context for this account. Tiering helps you know when to do a full read vs. a quick skim.

```
## Reading Strategy

**Tier 1 (Strategic / High-Risk Accounts):**
- Always read: _context.md (Identity, Health, Risks, Goals, Recent History)
- Then read: Key call-notes and recent interactions
- Then read: the Value/Risk/Goal Log section in _context.md for decision history and risk timeline
- Update frequency expectation: _context.md updated after every call; V/R/G Log appended by retros

**Tier 2 (Mid-Market / Healthy Accounts):**
- Always read: _context.md (Health, Risks, Goals)
- Skim: Recent History (last 2-3 entries)
- Details from: call-notes only if needed for context
- Update frequency expectation: _context.md updated monthly or after significant calls

**Tier 3 (SMB / Stable Accounts):**
- Quick read: Identity + Current Health sections only
- Skip: Recent History unless directly relevant
- Details from: call-notes only if preparing for QBR
- Update frequency expectation: _context.md updated quarterly or as needed

---

**This account is Tier {1|2|3}. Reading strategy above applies.**

**Why this tier?** {Explanation. Examples: "Tier 1: Renewal approaching, high churn risk, executive escalation active"; "Tier 2: Healthy, mid-market, standard cadence"; "Tier 3: Stable SMB, low maintenance"}

Example:
```
## Reading Strategy

**Tier 1 (Strategic / High-Risk Accounts):**
[Full Tier 1 instructions as above]

**This account is Tier 1. Reading strategy above applies.**

**Why this tier?** Renewal approaching (35 days), marked as high churn risk, budget uncertainty. Requires frequent executive engagement and context updates. CSA should review account before every customer call.
```

---

## Account Metadata

Quick reference facts.

```
## Account Metadata

| Field | Value |
|-------|-------|
| **Account ID** | {ID} |
| **ARR** | ${Amount} |
| **Renewal Date** | YYYY-MM-DD |
| **Support Tier** | {Tier} |
| **Primary AE** | {Name} ({Email}) |
| **CSA** | {Your Name} ({Email}) |
| **Internal Slack Channel** | #{channel-name} |
| **Customer Slack Channel** | #{channel-name} |
| **Jira Project** | {KEY} |
| **Salesforce Link** | [Link](https://...) |
```

Example:
```
## Account Metadata

| Field | Value |
|-------|-------|
| **Account ID** | 001234567890 |
| **ARR** | $150,000 |
| **Renewal Date** | 2026-11-30 |
| **Support Tier** | Premium |
| **Primary AE** | Sarah Chen (sarah.chen@company.com) |
| **CSA** | {CSM_NAME} ({CSM_EMAIL}) |
| **Internal Slack Channel** | #acme-corp |
| **Customer Slack Channel** | #acme-success |
| **Jira Project** | ACME |
| **Salesforce Link** | [Acme Corp](https://company.salesforce.com/001234567890) |
```

---

## Collaborators

People who have context on this account (co-CSMs, AEs, PMs, eng team, etc.). Update when team composition changes.

```
## Collaborators

| Name | Role | Email | Hub / Link | Hub Type | Notes |
|------|------|-------|------------|----------|-------|
| {Name} | {Role} | {Email} | {Link or "—"} | {Notion\|Drive\|None} | {Relationship to account} |
| {Name} | {Role} | {Email} | {Link or "—"} | {Notion\|Drive\|None} | {Relationship to account} |
```

Examples:
```
## Collaborators

| Name | Role | Email | Hub / Link | Hub Type | Notes |
|------|------|-------|------------|----------|-------|
| {CSM_NAME} | CSA (Owner) | {CSM_EMAIL} | — | — | Manages all CS decisions; primary customer contact |
| Sarah Chen | Account Executive | sarah.chen@company.com | — | — | Renewal owner; executive sponsor Michael Johnson is her peer |
| Jordan Rivera | Co-CSM (Shared Book) | jordan.rivera@company.com | [Co-Owner's Hub](https://notion.so/Co-Owner-Hub) | Notion | Co-manages 5 shared accounts in our book; authoritative for renewal strategy |
| John Doe | Product Owner (Customer) | john@acme.com | — | — | Executive sponsor; decision maker on product direction |
| Jane Smith | Product Manager (Customer) | jane@acme.com | — | — | Primary technical contact; replaced Sarah Kim in April 2026 |
```

---

## Sources

Data sources for this account. Where to find authoritative context about this customer.

Tells Janitor where to validate freshness and tells you where to find latest updates.

```
## Sources

| Source | Type | Location | Owner | Freshness Expectation | Notes |
|--------|------|----------|-------|----------------------|-------|
| {Name} | {auto\|manual} | {URL or path} | {Person} | {daily\|weekly\|monthly\|quarterly} | {Description} |
```

**Source Type:**
- **auto:** Automatically synced/monitored by Janitor (Notion, Drive, BQ, Slack, Jira)
- **manual:** Requires human update (CSV export, Salesforce notes, email summaries, etc.)

**Freshness Expectation:**
- **daily:** Updated daily (e.g., Slack channel activity, call notes from that day)
- **weekly:** Updated weekly (e.g., AE notes, weekly check-ins)
- **monthly:** Updated monthly (e.g., usage reports, renewal discussions)
- **quarterly:** Updated quarterly (e.g., QBR notes, business reviews)
- **as-needed:** Updated only when significant event occurs (e.g., risk escalation, goal change)

Example:
```
## Sources

| Source | Type | Location | Owner | Freshness Expectation | Notes |
|--------|------|----------|-------|----------------------|-------|
| _context.md | manual | /Accounts/Acme Corp/_context.md | {CSM_NAME} | Daily after calls | Working memory; most current snapshot |
| Notion Account Hub | auto | [Link](https://notion.so/acme-hub) | Jordan Rivera | Weekly | Co-owner's shared account hub; authoritative for renewal data |
| Drive folder | auto | [Acme Corp Folder](https://drive.google.com/...) | AE Sarah Chen | As-needed | Contracts, decks, analyses, one-pagers |
| Slack #acme-corp | auto | #acme-corp | {CSM_NAME} + team | Daily | Internal coordination, project updates |
| Slack #acme-success | auto | #acme-success | Acme team + us | Daily | Customer-facing comms, escalations, decisions |
| Jira ACME | auto | [ACME Project](https://jira.company.com/browse/ACME) | Our Eng | Weekly | Go-live project, implementation tasks, blockers |
| BigQuery sales_intelligence | auto | sales_intelligence.acme_corp | BQ admin | Daily | Usage data, feature adoption, NPS/CSAT (if available) |
| Salesforce Account | manual | [Acme Corp](https://salesforce.com/001234567890) | AE Sarah Chen | Monthly | Contract data, renewal dates, open opportunities |
| AE weekly notes | manual | Email or internal tool | AE Sarah Chen | Weekly | AE's customer updates, deal progress, executive notes |
```

---

## How to Use This Manifest

**When you're about to engage this account:**
1. Check Tier level (Tier 1 = detailed read, Tier 3 = quick skim)
2. Follow the Reading Strategy for that tier
3. Check Collaborators to see who else is involved
4. Check Sources to know where to find latest data

**When you're updating account context:**
1. Update the appropriate section in _context.md
2. If you're adding a new data source (e.g., a new Notion page), add it to the Sources table
3. Update Last Updated timestamp in header

**When Janitor runs:**
1. Janitor reads this manifest
2. For each source, checks its freshness (compares Last Updated in that source to the expectation)
3. Flags stale sources in hygiene report
4. Validates that source links still resolve

---

## Example Complete Manifest

```
# Acme Corp — MANIFEST

**Account Tier:** Enterprise  
**Reading Priority:** Tier 1 (Strategic, high churn risk, renewal at-risk)  
**Created:** 2024-06-15  
**Last Updated:** 2026-04-10  
**Maintained By:** {CSM_NAME}

---

## Reading Strategy

**Tier 1 (Strategic / High-Risk Accounts):**
- Always read: _context.md (Identity, Health, Risks, Goals, Recent History)
- Then read: Key call-notes and recent interactions (last 5 calls)
- Then read: the Value/Risk/Goal Log section in _context.md for decision history and risk timeline
- Update frequency expectation: _context.md updated after every call; V/R/G Log appended by retros

**This account is Tier 1.**

**Why this tier?** Renewal 2026-11-30 (35 days away). Marked as churn risk HIGH. Budget uncertainty due to customer's internal migration. CFO (Michael Johnson) and AE (Sarah Chen) both flagged renewal as uncertain. Requires weekly executive engagement and fresh context updates.

---

## Account Metadata

| Field | Value |
|-------|-------|
| **Account ID** | 001234567890 |
| **ARR** | $150,000 |
| **Renewal Date** | 2026-11-30 |
| **Support Tier** | Premium (24/7, dedicated support) |
| **Primary AE** | Sarah Chen (sarah.chen@company.com) |
| **CSA** | {CSM_NAME} ({CSM_EMAIL}) |
| **Internal Slack Channel** | #acme-corp |
| **Customer Slack Channel** | #acme-success |
| **Jira Project** | ACME |
| **Salesforce Link** | [Acme Corp](https://company.salesforce.com/001234567890) |

---

## Collaborators

| Name | Role | Email | Hub / Link | Hub Type | Notes |
|------|------|-------|------------|----------|-------|
| {CSM_NAME} | CSA (Owner) | {CSM_EMAIL} | — | — | Primary CS contact; owns all context updates |
| Sarah Chen | Account Executive | sarah.chen@company.com | — | — | Renewal owner; driving executive alignment |
| Jordan Rivera | Co-CSM (Shared Book) | jordan.rivera@company.com | [Co-Owner's Hub](https://notion.so/Co-Owner-Hub) | Notion | Provides authoritative input on renewal strategy for shared accounts |
| Michael Johnson | CFO (Customer) | mike@acme.com | — | — | Budget approver; key renewal stakeholder |
| John Doe | VP Product (Customer) | john@acme.com | — | — | Executive sponsor; decision maker |
| Jane Smith | Product Manager (Customer) | jane@acme.com | — | — | Primary technical contact; replaced Sarah Kim April 2026 |

---

## Sources

| Source | Type | Location | Owner | Freshness Expectation | Notes |
|--------|------|----------|-------|----------------------|-------|
| _context.md | manual | /Accounts/Acme Corp/_context.md | {CSM_NAME} | Daily after calls | Working memory; update after every customer interaction |
| Notion Account Hub | auto | [Acme Corp Hub](https://notion.so/acme-hub) | Jordan Rivera | Weekly | Co-owner's authoritative hub for shared account renewal data |
| Drive folder | auto | [Acme Corp Folder](https://drive.google.com/drive/folders/acme-folder-id) | AE Sarah Chen | As-needed | Contracts, SOWs, decks, analyses; shared with AE |
| Slack #acme-corp | auto | #acme-corp | {CSM_NAME} + team | Daily | Internal team coordination, implementation notes, escalations |
| Slack #acme-success | auto | #acme-success | Customer + our team | Daily | Customer-facing shared channel; decisions, updates, questions |
| Jira ACME | auto | [ACME Project](https://jira.company.com/browse/ACME) | Our Eng team | Weekly | Data Warehouse Phase 1 implementation; go-live tasks, blockers |
| BigQuery sales_intelligence | auto | acme_corp table in sales_intelligence dataset | BQ admin | Daily | Usage metrics, feature adoption, monthly active users |
| Salesforce Account Record | manual | [Acme Corp](https://salesforce.com/001234567890) | AE Sarah Chen | Monthly | Contract terms, renewal date, associated opportunities |
| AE weekly customer notes | manual | Email or Salesforce Chatter | AE Sarah Chen | Weekly | Sarah's executive updates, customer sentiment, business development notes |

```

---

## End of Template

Save this as `/Accounts/[Account Name]/_MANIFEST.md`.

**Key point:** This is your reading guide. Every CSA should skim this before engaging the account. Janitor uses Sources table to audit data freshness.

**Keep it current:** When you add a new data source (Notion page, Drive folder, Slack channel), add it to the Sources table so Janitor can monitor it.

