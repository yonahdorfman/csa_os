---
name: Harvest Mixpanel
version: 2.0
last_updated: 2026-04-29
category: sub-skill
requires_mcp: []
optional_mcp: [BigQuery, Mixpanel MCP]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_NAME: "Your name"
output: Product telemetry signals per account
invoke:
  - Called by harvest-all during daemon cycle
  - "/harvest-mixpanel"
---

# Harvest Mixpanel

Pull product telemetry for your accounts. Two modes: BQ plan data (always
available) and Mixpanel MCP real-time queries (when approved).

## Data Architecture (Verified April 29, 2026)

**Mixpanel Project 3** ("Mixpanel (project 3)") is Mixpanel's own product
analytics. It tracks how ALL customers use Mixpanel. The join key between
your accounts and Project 3 data is:

**Property:** `Project owner billing account ID`
- Display name: `Billing Account ID`
- Type: `number`
- Resource: `event` (exists on most product events)
- Example value: `123457708008`

**BQ source:** `account_metadata.mixpanel_billing_account_id` per account.

### Account → Billing ID Map (Verified)

| Account | Billing Account ID |
|---------|-------------------|
| Workday | 2981837 |
| DocuSign | 3789 |
| Farmers Insurance | 1072464 |
| OpenTable | 10621 |
| Pearson Global | 11425 |
| ATB Financial | 1556049 |
| Take-Two Interactive | 697665 |
| Menards | 5852037 |
| Yelp | 2425111 |
| ZipRecruiter | 513975 |
| Store No 8 | 3602575 |

### Workspaces

Project 3 has 3 workspaces:
- **79: Mixpanel Customer Data** (default) — excludes @mixpanel.com users. USE THIS.
- 75: All Project Data — includes internal usage
- 84: Dogfood Workspace — @mixpanel.com only

Always query against workspace 79 to get customer-only data.

## Mode A: BQ Plan Data (always available)

```sql
SELECT
  account_name, mixpanel_billing_account_id,
  plan_name, plan_percent_used,
  health_score_vhs, health_score_adoption,
  health_score_depth, health_score_breadth,
  health_score_utilization, health_score_lexicon
FROM `mixpanel-sa.sales_intelligence.account_metadata`
WHERE LOWER(csa) LIKE '%{csm_name_lower}%'
ORDER BY plan_percent_used DESC
```

### Signals from BQ

| Condition | Signal | Urgency |
|-----------|--------|---------|
| `plan_percent_used > 100` | 🔴 OVERAGE | critical |
| `plan_percent_used > 80` | ⚠️ Approaching limit | high |
| `plan_percent_used < 20` | Low utilization | normal |
| `health_score_adoption = 0` | Zero feature adoption | high |
| `health_score_vhs < 4` | VHS critically low | high |

---

## Mode B: Mixpanel MCP (Project 3, real-time)

**Requires:** Mixpanel MCP connected + Run-Query permission approved.
**Project:** 3
**Workspace:** 79 (Mixpanel Customer Data)

### Available Events for Customer Usage Tracking

These events exist on Project 3 and carry the `Project owner billing account ID`
property, meaning they can be filtered per customer:

**Core Usage:**
- `Viewed report` — user viewed any report type
- `Report Loaded` — report rendered with data
- `[server] Visited Report` — server-side report visit
- `[ReportManager] Core Report Query` — user ran a query
- `[ReportManager] Core Report Save` — user saved a report
- `Query` — any analytics query executed

**Feature Adoption:**
- `[Copilot] Prompt Sent` — MCP/Copilot usage
- `[Copilot] Response Received` — MCP response delivered
- `[Copilot] Message Thumbs Up Clicked` — positive MCP feedback
- `[Copilot] Message Thumbs Down Clicked` — negative MCP feedback
- `[Recording Player] Replay Watched` — Session Replay usage
- `[Recording Player] Replay List Sorted` — Session Replay list interaction
- `[Recording Player] Modal Opened` — Session Replay detail view
- `[Web Dash] Dashboard loaded` — dashboard views
- `[Web Dash] Add card` — dashboard creation activity
- `[Web Dash] Save changes` — dashboard edits
- `[Custom Alerts] Created custom alert` — alert setup
- `[Custom Alerts] Updated custom alert` — alert management
- `$mp_web_page_view` — any Mixpanel page view
- `[AI] Spark Report Generated` — AI-generated reports

**Account/Admin:**
- `User Account Created` — new user onboarded
- `Organization Created` — new org created
- `[Feature Flag View Page] commit feature flag` — experimentation usage
- `[Experiment Page] launch` — experiment launched
- `[Experiment List Page] list loaded` — experiments feature used
- `[Canvases] Created` — canvas creation

**Collaboration:**
- `Create Comment` — commenting on reports
- `Reply to Comment` — threaded discussion
- `[Annotations] Add Annotation` — timeline annotations
- `Click Notification` — notification engagement
- `Saved Metric` — metric tree engagement

### Query Patterns

All queries follow this pattern — filter by billing account ID per customer:

**WAU (Weekly Active Users) — last 4 weeks:**
```json
{
  "name": "{account} WAU 4wk",
  "chartType": "line",
  "unit": "week",
  "dateRange": {"type": "relative", "range": {"unit": "week", "value": 4}},
  "metrics": [{
    "eventName": "Viewed report",
    "measurement": {"type": "basic", "math": "wau"},
    "filters": [{
      "type": "number",
      "propertyName": "Project owner billing account ID",
      "propertyType": "number",
      "resource": "event",
      "operator": "is equal to",
      "value": {BILLING_ID}
    }]
  }]
}
```

**Total unique users — last 30 days:**
```json
{
  "name": "{account} unique users 30d",
  "chartType": "bar",
  "dateRange": {"type": "relative", "range": {"unit": "month", "value": 1}},
  "metrics": [{
    "eventName": "Viewed report",
    "measurement": {"type": "basic", "math": "unique"},
    "filters": [{
      "type": "number",
      "propertyName": "Project owner billing account ID",
      "propertyType": "number",
      "resource": "event",
      "operator": "is equal to",
      "value": {BILLING_ID}
    }]
  }]
}
```

**Feature adoption breakdown — what features are they using:**
```json
{
  "name": "{account} feature usage 30d",
  "chartType": "bar",
  "dateRange": {"type": "relative", "range": {"unit": "month", "value": 1}},
  "metrics": [
    {"eventName": "[ReportManager] Core Report Query", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Copilot] Prompt Sent", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Recording Player] Replay Watched", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Web Dash] Dashboard loaded", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Custom Alerts] Created custom alert", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Experiment Page] launch", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]}
  ]
}
```
Where `BILLING_FILTER` = the standard billing ID filter object above.

**MCP/Copilot adoption — are they using AI features:**
```json
{
  "name": "{account} MCP usage 30d",
  "chartType": "line",
  "unit": "week",
  "dateRange": {"type": "relative", "range": {"unit": "month", "value": 1}},
  "metrics": [
    {"eventName": "[Copilot] Prompt Sent", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Copilot] Response Received", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Copilot] Message Thumbs Up Clicked", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Copilot] Message Thumbs Down Clicked", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]}
  ]
}
```

**Session Replay adoption:**
```json
{
  "name": "{account} Session Replay 30d",
  "chartType": "bar",
  "dateRange": {"type": "relative", "range": {"unit": "month", "value": 1}},
  "metrics": [
    {"eventName": "[Recording Player] Replay Watched", "measurement": {"type": "basic", "math": "unique"}, "filters": [BILLING_FILTER]},
    {"eventName": "[Recording Player] Replay Watched", "measurement": {"type": "basic", "math": "total"}, "filters": [BILLING_FILTER]}
  ]
}
```

### Recommended Query Set per Account

Run these 5 queries per account during each harvest cycle:

1. **WAU trend** (4 weeks) — is usage growing or declining?
2. **Unique users** (30 days) — how many people are active?
3. **Feature adoption** (30 days) — which features are they using?
4. **MCP/Copilot usage** (30 days) — are they adopting AI features?
5. **Session Replay** (30 days) — adoption of newer features?

For a 10-account book, that's 50 queries. Run sequentially with a brief
pause between each to avoid rate limits.

---

## Output Format

```markdown
## Mixpanel Harvest — {date}

**Mode:** {BQ only | BQ + MCP}
**Accounts scanned:** {N}

### Alerts

🔴 **Menards** — OVERAGE: 627% of plan
⚠️ **Farmers** — Approaching limit: 97%

### Per-Account Telemetry

#### {Account Name} (billing: {id})
- **Plan usage:** {X}% | VHS: {score} | Adoption: {score}
{if MCP data available:}
- **WAU (4wk):** {w1} → {w2} → {w3} → {w4} ({trend})
- **Unique users (30d):** {N}
- **Top features:** Reports ({N}), Dashboards ({N}), Copilot ({N}), SR ({N})
- **MCP adoption:** {N} prompts, {thumbs_up}👍 / {thumbs_down}👎
- **Signal:** {interpretation — growing/stable/declining, what's notable}
```

## Writing to Context

Update the **Mixpanel Telemetry** subsection within Sales Intelligence:

```
### Mixpanel Telemetry
- **Billing ID:** {id}
- **Plan usage:** {X}% ({plan_name summary})
- **VHS:** {score} | **Adoption:** {score}
{if MCP data:}
- **WAU trend (4wk):** {w1} → {w2} → {w3} → {w4} ({direction})
- **Active users (30d):** {N}
- **Features used:** {list with counts}
- **MCP prompts (30d):** {N} ({thumbs_up}👍 / {thumbs_down}👎)
- **Session Replay (30d):** {N} users, {N} replays
- **Last refreshed:** {date}
```

Flag overages and adoption anomalies as signals for the compress step.

## Graceful Degradation

- Neither BQ nor MCP → skip, log `harvest-mixpanel:skipped:no-source`
- BQ available, MCP not → Mode A only
- MCP available but Run-Query blocked → log warning, fall back to Mode A
- MCP rate limited → complete what you can, fall back to Mode A for remaining
- Billing ID missing for an account → skip that account for MCP queries

## MCP Permission Notes

As of April 29, 2026: `Run-Query` requires approval in the Mixpanel MCP
connector settings. `Get-Projects`, `Get-Events`, `Get-Properties` work
without additional approval. `Get-Business-Context` requires `business_context`
scope. Check with your Mixpanel admin if Run-Query is blocked.
