Work as if it is Tue 2026-08-25 08:00 (America/Los_Angeles), and as if
you are Account Assistant, the agent for Acme, maker of Relay. The
fixtures below replace every outside read and their formats are
authoritative: work from them, not from live systems or the knowledge
files on disk.

The slack-post skill is not loaded in rehearsal; its contract, condensed
but authoritative: one chat.postMessage payload with `channel`, a `text`
fallback leading with the day's headline (never a generic label), and
`blocks`. Slack mrkdwn, not markdown: links `<url|text>`, bold
`*text*`, literal `•` bullets. No @channel, no @here. One message,
maximum.

$BRIEF_CHANNEL is C0AB12CD3.

## Fixtures

`./changes.md`:

```markdown
# Pending knowledge changes

- Routine: account-brief
- From: 3a91c0de77b2
- Through: 8f4e2b1c9d07

## 2026-08-24 Run upcoming-touches (run_t7q2m9x1ab): completed

### events.md

+ - 2026-08-24 07:41 upcoming-touches: prepared Brightline Health QBR (Tue 08-25, Dana Okafor) — note https://app.attio.com/acme/notes/n_881 ; angle: 148 active users on 120 seats, and Joanne's SAML question (security review end of Sept) is still unanswered in #c-brightline — bring a date or a plan.
+ - 2026-08-24 07:44 upcoming-touches: Kestrel SSO follow-up (Priya Nair) overdue since 08-20; asked.

### tasks.md

+ - [ ] `task-20260824-1` Kestrel Logistics: the SSO follow-up task is 4 days overdue and Kestrel's IT contact changed — reschedule it or hand it off. (raised by upcoming-touches 2026-08-24, for Priya Nair)
+ - [ ] `task-20260824-2` Harbor & Finch intro call Wed 08-26 has no owner — who takes it? (raised by upcoming-touches 2026-08-24)

## 2026-08-24 Run renewals-and-expansion (run_r2v8k3n6cd): completed

### events.md

+ - 2026-08-24 08:31 renewals-and-expansion: Kestrel Logistics (renews 10-15) routine → at risk: health amber, active users 41→27, SSO stalled, IT contact changed, no touch scheduled. Read: https://app.attio.com/acme/notes/n_884
+ - 2026-08-24 08:33 renewals-and-expansion: Pinecrest Schools entered the horizon (renews 11-18): at risk — 52 active on 40 seats but an unresolved SSO login outage (3rd ticket) and no human touch since May. Read: https://app.attio.com/acme/notes/n_885
+ - 2026-08-24 08:36 renewals-and-expansion: Westbrook Dental renewal date passed 08-12, still open, no owner; asked.

### tasks.md

+ - [ ] `task-20260824-3` Kestrel Logistics is at risk with no conversation scheduled before the Oct 15 renewal — schedule one. (raised by renewals-and-expansion 2026-08-24, for Priya Nair)
+ - [ ] `task-20260824-4` Westbrook Dental's renewal date passed Aug 12 and nobody owns it — renewed, lapsed, or in negotiation? (raised by renewals-and-expansion 2026-08-24)

## 2026-08-24 Run support-watch (run_s5w1p8q2ef): completed

### events.md

+ - 2026-08-24 13:12 support-watch: Pinecrest Schools escalating — Ruth Alvarez (IT Director) asked for a call today, board meeting Wed; staff SSO login still broken since Thu; waiting 45h. https://secure.helpscout.net/conversation/4412/4412
+ - 2026-08-24 13:14 support-watch: Harbor & Finch signal — Mateo Lind asked whether Relay has a routing-rules API; says they're deciding between building on Relay or moving to Front next quarter. https://secure.helpscout.net/conversation/4421/4421

### tasks.md

+ - [ ] `task-20260824-5` Pinecrest Schools: Ruth Alvarez wants a call today about the SSO outage (3rd ticket, board meeting Wednesday) — someone should call her, not email. (raised by support-watch 2026-08-24, for Sam Reyes)

## 2026-08-25 Run health-shifts (run_h9c4d2z7gh): completed

### events.md

+ - 2026-08-25 07:06 health-shifts: Kestrel Logistics amber → red (score 54→38, active 27→19), inside renewal horizon; already at risk per yesterday's read. Pinecrest Schools green → amber (79→61), explained by the open SSO outage.
+ - 2026-08-25 07:07 health-shifts: Marigold, Brightline, Harbor & Finch steady. NO-OP.

## 2026-08-25 Run customer-channels (run_c3n7m1k5ij): completed

### events.md

+ - 2026-08-25 07:20 customer-channels: Brightline — Dana said Friday she'd have a SAML answer Monday; Joanne's ask (08-20) still open. https://acme.slack.com/archives/C0BR1/p1755712920000000
+ - 2026-08-25 07:21 customer-channels: Kestrel — Ben Oyelaran (new IT lead, replacing Claire) asked Saturday where the SSO setup stands; unanswered 2 days. https://acme.slack.com/archives/C0KE2/p1755878200000000
+ - 2026-08-25 07:22 customer-channels: Harbor & Finch — Mateo asked Sam to add Dana to the channel (their CEO wants to meet the account owner); not done. https://acme.slack.com/archives/D0HF3/p1755813600000000
```

`knowledge/ledgers/account-brief.md` does not exist — you have never
delivered a brief.

## Output

Print, and nothing else:

1. The exact chat.postMessage payload you would send, as JSON, verbatim.
2. Your consume decision, and why.
3. Decision notes: what you left out of the brief and why.
