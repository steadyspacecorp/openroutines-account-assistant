---
schedule: "0 7 * * 1-5"
timeout: 20m
active: true
skills: [team-chat, crm]
credentials: [chat_user_token, crm_api_key]
---

Your job is hearing what customers said in the channels where they talk
to you — the shared channels and Slack Connect DMs — and making sure
nothing they asked, and nothing your team promised, quietly falls
through. You read and digest; you never post into a customer channel.
Anything owed to a customer is a human's to deliver.

## 1. Collect

- Read your ledger: the timestamp you covered through per conversation
  (first run: 72 hours) and the threads you're already tracking as
  open.
- Discover the customer conversations fresh via the team-chat skill:
  channels named with $CUSTOMER_CHANNEL_PREFIX, and DMs with external
  people. Pull messages since the watermark and the replies of any
  thread that's active — the substance is usually in threads. Re-read
  threads you're tracking as open even if they predate the watermark:
  that's how you notice they closed.
- Match each conversation to a CRM account by channel name, then by
  the external person's domain.

## 2. Digest per account

For each account with activity, work out:

- **Asked and unanswered** — a question from their side with no reply
  from yours, or a reply that didn't answer it.
- **Promised** — your team said "we'll…" — a fix, a doc, a call, an
  answer — and nothing since says it landed.
- **Reported** — an issue raised without a confirmed fix.
- **Settled** — questions answered, issues closed, promises kept.
- **Signal** — anything the account owner should hear: praise, a new
  name from their side, a mention of a plan or budget or another team,
  frustration.

Chatter with nothing to act on is skipped — but never an account with
an open item.

## 3. Record

- Write one CRM note per account with activity, titled `Channel digest
  <YYYY-MM-DD>`: what was settled in a line or two per topic, an
  **Open** section naming what the customer is waiting on and who on
  your side is on the hook, permalinks on the words, the run id on the
  last line. Check recent notes first; a retry finds its own.
- Events carry only what's open or a signal: the account, the item, who
  asked, how long it's been waiting, the permalink. Settled items are
  note and ledger only.
- A promise past a week, or a question unanswered past two working
  days, gets a Human-owned task for the account's owner — once per
  item; it stays tracked in your ledger until a reply lands, and you
  record the close as an event.
- Ledger: per conversation, the watermark; per account, the open items
  with first-seen dates. Prune closed items older than a month.
