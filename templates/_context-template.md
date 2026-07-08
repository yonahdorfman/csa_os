# {Account Name}

**Last Updated:** {YYYY-MM-DD HH:MM} | **Updated By:** {who or what updated this}
**Why Updated:** {one-line reason — what changed and why}
**Previous:** {one-line summary of the last state, so you can see the delta}

---

## Identity
- ARR: ${amount} | Renewal: {date} ({days} days)
- Plan: {plan type} | Contract: {terms, notice window}
- FY{XX} Status: {Active|Pending-Upsell|Renewal|At-Risk} | Churn Risk: {Low|Medium|High} ({why}) | Shared with {AE}: {Y|N}

---

## Key Contacts

Each contact should have: name, title, role in the relationship, email, and last-touched date.
Contacts are people, not interaction counts. Titles and roles matter because they tell you
who to escalate to, who the champion is, who holds budget, and who does the implementation work.

- {Name} — {Title} — {Role in relationship} ({email}) — last touched: {date}
- {Name} — {Title} — {Role in relationship} ({email}) — last touched: {date}
- {Name} — {Title} — {Role in relationship} ({email}) — last touched: {date}

---

## Current Health

One paragraph of human-readable assessment. Not a score dump — an interpretation.
What's working, what's at risk, what's the trajectory. Include the structural dynamics
(e.g., "DS/PM bottleneck limits adoption ceiling" or "deeply embedded in core workflow,
low switching cost concern").

---

## Active Risk

Dated entries with analysis, not just labels. Each risk should explain WHY it's a risk,
not just THAT it's a risk. Include the business impact.

- **{YYYY-MM-DD}:** {Risk description} — {analysis of why this matters and what it could lead to}
- **{YYYY-MM-DD}:** {Risk description} — {analysis}

---

## Active Goal

The strategic lever you're working. Not a wish list — the ONE thing (or 2-3 max)
that would change the account trajectory. Include the target state.

- **{YYYY-MM-DD}:** {Goal} — {target state, how you'd measure success, why it matters for renewal/upsell}

---

## Open Items

Checklist with urgency markers, due dates, owners, and status. Overdue items
should be immediately visible. Use ⚠️ for items that need attention today/this week.

- [x] ⚠️ {Completed item} — {outcome if notable}
- [ ] ⚠️ {Urgent item} — {owner} — {due date} — {status}
- [ ] {Normal item} — {owner} — {due date}
- [~] ~~{Archived item}~~ — ARCHIVED ({date}): {why}

---

## Recent History

Newest first. Each entry should include: date, what happened, source, and implications.
Don't just log events — interpret them. "Call with Chad" is useless. "Call with Chad —
all events fix confirmed for May, MCP security review underway" tells you what it means.

Include source attribution (Slack, Gmail, Calendar, Statisfy, DM note, etc.) so retros
can trace where information came from.

- {YYYY-MM-DD}: {What happened + interpretation + source}
- {YYYY-MM-DD}: {What happened + interpretation + source}

---

## Value/Risk/Goal Log

Append-only, dated log. Replaces the old Brief file. Retros, briefings,
and dashboards read from this section. Never edit or delete old entries.

Entries are tagged VALUE, RISK, or GOAL for filtering at read time:
- QBR narratives pull VALUE entries
- Risk dashboards pull RISK entries
- Goal tracking pulls GOAL entries

**{YYYY-MM-DD} | VALUE | {summary}**
- {what value was delivered, for whom, impact}
- Source: {retro|briefing|DM note|harvester}

**{YYYY-MM-DD} | RISK | {summary}**
- {risk description, business impact, mitigation status}
- Source: {retro|briefing|DM note|harvester}

**{YYYY-MM-DD} | GOAL | {summary}**
- {goal milestone, progress update, next step}
- Source: {retro|briefing|DM note|harvester}

---

## Sales Intelligence
> 🤖 Machine-generated — BQ data as of {date}. Do not edit manually; this section is overwritten on each refresh.
> SFDC Account ID: `{id}` | BQ ARR: ${amount} | Renewal: {date} | Auto-renew: {Yes|No}

### Health & Sentiment
- **Overall:** {rating} | **Sentiment:** {label} ({score})
- **Exec support needed:** {yes/no/monitor}
- **Top priority:** {one line}
- **Assessment:** {2-3 sentence assessment from BQ}
- **Usage:** {usage patterns}
- **Support health:** {recent tickets or "no open tickets"}
- **Conversation health:** {interaction quality assessment}

### Active Opportunity
- **{Opp name}** — ${arr} ARR — Stage: {stage} — Close: {date}
- **Account plan:** {gdoc link}

### BQ Risks
- ⚠️ {Risk from BQ with mitigation}

### Strategic Goals (Mixpanel Alignment)
- **{Goal}:** {description}
  - *Mixpanel angle:* {how Mixpanel connects to this goal}

### Open Action Items (BQ)
- 🔄 **{Task}** — {owner} — due {date}
  → {link to ticket or call}

### Top Contacts by Interaction (BQ)
- **{Name}** — {N} interactions

### Recent Support Tickets (last 60d)
{tickets or "No open tickets in the last 60 days."}

### Executive Summary (BQ)
{2-3 sentence summary of pipeline, last interaction, key activities}
