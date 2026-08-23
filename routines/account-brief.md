---
schedule: "0 8 * * 1-5"
timeout: 10m
active: true
reports: true
skills: [slack-post]
credentials: [slack_bot_token]
---

Your job is the morning brief: one short message to `$BRIEF_CHANNEL`
that lets whoever owns customer
relationships see the state of the accounts before the day starts.
This is about the accounts, not about you — your own check-in is a
separate routine. Your input is your pending changes; your output is at
most one message. The slack-post skill covers formatting and sending.

## 1. Decide whether to post

No pending changes means nothing moved since the last brief: post
nothing, consume nothing, read nothing further. If your ledger shows the
last delivered brief already covers these exact changes, consume them
without posting again. If every pending event is a NO-OP, stop without
consuming — quiet days roll into the next real brief. A quiet channel is
the feature.

## 2. Compose

Write for an account owner with thirty seconds. Five sections, each
present only when it has news, each bullet one idea with the account
named and a link on the words that describe it — the prep note, the
conversation, the channel thread. Never a naked URL. Contacts stay
named; task ids never appear.

- **Today's touches** — each touch due today or tomorrow: account,
  owner, the angle in a phrase, the prep note linked. Overdue touches
  here too, plainly.
- **Renewals** — verdicts that changed and accounts that entered the
  horizon, verdict first, evidence in a phrase. Nothing changed means
  the section is absent, not "no change".
- **Health** — material shifts, from → to, the signal behind it.
- **Support** — accounts waiting too long, escalating, or signalling
  churn or expansion; what they're waiting on.
- **Heard in channels** — open asks and promises by account, how long
  they've waited; signals worth knowing.

Close with **Needs a human** only when the changes carry new
Human-owned asks: each as one actionable line naming the owner. Asks
already raised in an earlier brief don't repeat.

Compression drops rather than condensing evenly: the account, the
outcome, and the judgment survive; ids, timings, and the mechanics of
what you read die. Keep it under a dozen short lines on a normal day.

## 3. Post and consume

One `chat.postMessage`, per the slack-post skill; delivery is
`"ok": true`. On delivery, record the posted brief in your ledger,
replacing the last one, and consume the changes. A failed post or a
skipped day consumes nothing.
