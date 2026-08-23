---
name: customer-health
description: Read the team's customer health report — one row per account with a score or tier and the signals behind it — from a URL or a CRM attribute, so health-shifts can diff it run over run.
---

# Customer health

Every team scores health differently — a number, a color, a tier, a
handful of usage signals. This skill doesn't care how; it cares that the
report can be read as **one row per account, as of a date**, and that
the same row means the same thing next week. `$HEALTH_SOURCE` says
where it lives.

## Shapes

**A URL** (`https://…`) returning CSV or JSON. Fetch it with curl —
add an auth header only if the team documents one in
knowledge/context.md. Expected columns/keys, by whatever names the
report uses: an account identifier (name or domain), a score or tier,
and optionally the signals behind it (active users, seats used,
last-active, feature adoption). On your first run, read the header,
decide which column is which, and record that mapping in your ledger.

**A CRM attribute** (`crm:<attribute-slug>`) on each company record —
a health score or status the team maintains in the CRM. Read it with
the `crm` skill (query companies, read the attribute; Attio attributes
with history expose when they last changed).

## Reading it well

- Match rows to CRM accounts by domain first, name second; a row that
  matches no account goes in your run output, not in knowledge.
- A report that won't fetch, returns an empty body, or changes shape
  gets no diff: leave your last snapshot alone, note it in the ledger,
  and raise one Human-owned task if it stays broken across runs.
- "Major shift" is the routine's call, but the raw material is here:
  the score's delta, the tier change, and which signal moved. Record
  enough in your ledger to explain a verdict later — per account, the
  last value you saw and the date.
