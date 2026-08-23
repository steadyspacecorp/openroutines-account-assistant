Work as if it is Tue 2026-08-25 07:00 (America/Los_Angeles), and as if
you are the account assistant for Acme, maker of Relay. $HEALTH_SOURCE
is a URL returning CSV. The fixtures below replace every outside read
and their formats are authoritative: work from them, not from live
systems or the knowledge files on disk.

## Fixtures

Your ledger: column mapping settled 2026-08-03 (`account` = domain,
`tier` = green/amber/red, `score` 0–100, `active_30d`, `seats`,
`last_active`). Readings through Mon 2026-08-24:

| account | tier | score | active_30d | seats | last_active |
|---|---|---|---|---|---|
| brightline.health | green | 88 | 148 | 120 | 2026-08-24 |
| kestrel.io | amber | 54 | 27 | 60 | 2026-08-22 |
| marigold.coop | green | 81 | 22 | 25 | 2026-08-24 |
| pinecrestschools.org | green | 79 | 52 | 40 | 2026-08-24 |
| harborfinch.com | green | 72 | 18 | 20 | 2026-08-24 |

Kestrel's prior readings: 08-18 amber/54/27; 08-11 green/71/41; 08-04
green/74/43. Threshold notes: "score move ≥ 20 or tier change = material."

Today's report:

```csv
account,tier,score,active_30d,seats,last_active
brightline.health,green,90,151,120,2026-08-25
kestrel.io,red,38,19,60,2026-08-23
marigold.coop,green,80,22,25,2026-08-25
pinecrestschools.org,amber,61,49,40,2026-08-25
harborfinch.com,green,74,21,20,2026-08-25
oakridge.example,green,66,9,10,2026-08-25
```

CRM: Kestrel Logistics renews 2026-10-15 (inside the 90-day horizon),
owner Priya Nair; no note or task this week explains a drop. Pinecrest
Schools renews 2026-11-18, owner Sam Reyes; an open support
conversation since 08-20 reports staff unable to log in via the new
district SSO. oakridge.example matches no CRM account.

## Output

Print, and nothing else:

1. Each account's verdict: material shift or not, and why.
2. The events you would record, verbatim.
3. Every Human-owned task you would raise, verbatim, with its assignee.
4. The ledger after the run, and what you'd say about oakridge.example.
