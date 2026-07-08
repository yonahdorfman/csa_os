# Gmail Matching Logic — Updated 2026-07-07

## Overview

The Gmail harvest process now uses a **domain-first matching strategy** with automatic new contact and domain detection.

## How It Works

### Step 1: Load Domain Registry (Lightweight)
- Reads the **Domains** section from each account's `_MANIFEST.md`
- Builds a simple lookup table: `domain → account name`
- Example: `altshul.co.il → Altshuler Shaham`
- **Does NOT store individual contact emails** — only domains for performance

### Step 2: Process Incoming Email
For each email in the past 72 hours (was 24 hours):

#### Domain Match
```
sender@altshul.co.il
         ↓ extract domain
    altshul.co.il
         ↓ lookup in registry
    Altshuler Shaham ✓
```

**Result:** Match to account (doesn't matter if sender is new or existing contact)

#### New Domain Detection
If domain doesn't match any account AND looks customer-related:
```
someone@potentialcustomer.com
         ↓ domain not in registry
         ↓ not gmail/outlook/common provider
         ↓ not automated (noreply@, etc.)
    FLAG: Potential New Customer Domain
```

**Output:** Lists in "New Domains to Review" section

## Improvements from Previous Version

| Before | After |
|--------|-------|
| Only unread emails | **All emails** (read + unread) |
| 24 hour window | **72 hour window** |
| 50 email limit | **100 email limit** |
| Fuzzy account matching | **Domain-only matching** |
| Heavy: stored all contact emails | **Lightweight: only stores domains** |
| No new domain flagging | **Flags potential customer domains** |

## Example Scenarios

### Scenario 1: Any Email from Known Domain
**Email:** `sarik@altshul.co.il` OR `newemployee@altshul.co.il`
- Domain: `altshul.co.il` → Altshuler Shaham ✓
- **Result:** Signal for Altshuler Shaham (doesn't check individual contact)

### Scenario 2: Unknown Domain, Looks Customer-Related
**Email:** `cto@startup-company.io`
- Domain: `startup-company.io` → no match ✗
- Not common provider ✓
- Not automated ✓
- **Result:** Flagged as "New Domain to Review"

### Scenario 3: Unknown Domain, Common Provider
**Email:** `randomuser@gmail.com`
- Domain: `gmail.com` → no match ✗
- Is common provider ✓
- **Result:** Listed in "Unmatched" (not flagged as new domain)

## Excluded Domains (Not Flagged as New)
- gmail.com, googlemail.com
- outlook.com, hotmail.com, live.com
- yahoo.com, ymail.com
- mixpanel.com (internal)
- Any email starting with: noreply@, no-reply@, notifications@, donotreply@

## Workflow Integration

1. **Harvest runs** (every tick during work hours)
2. **Matches emails** using domain-first logic
3. **Outputs:**
   - Regular signals for known accounts
   - New contacts to add to manifests
   - New domains to review/classify
4. **Compress skill** can use this to:
   - Update account contexts
   - Prompt CSM to add new contacts
   - Flag potential new accounts for investigation
