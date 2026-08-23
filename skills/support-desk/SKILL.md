---
name: support-desk
description: Read customer conversations from the support tool — recent conversations in a window, one thread — for account awareness. Read-only. Written for Help Scout; adapt for Intercom, Zendesk, Front, or whatever holds your conversations.
---

# Support desk

Routines read the support inbox to know what a customer is waiting on
and how the relationship is feeling. They never write to it: no replies,
no status changes, no tags. Answering customers is support's job; this
agent's job is noticing when an account needs an owner's attention.

Authentication is ambient. Declared as `oauth2_client` in
`openroutines.yml`, the stored secret is exchanged at `token_url` each
run and only the short-lived bearer arrives, as `$SUPPORT_DESK_TOKEN`. A
tool that uses plain API keys skips the typing: set the key as a raw
credential and it arrives verbatim as `$SUPPORT_DESK_SECRET`. Never
print, inspect, or write either anywhere.

## Help Scout (worked example — replace with your tool)

```bash
AUTH="Authorization: Bearer $SUPPORT_DESK_TOKEN"
BASE=https://api.helpscout.net/v2
```

- Mailboxes: `GET $BASE/mailboxes` — `$SUPPORT_MAILBOXES` holds the ids
  to watch (space-separated; empty means all).
- Conversations in a window, per status — never `status=all`, which
  includes spam:
  `GET $BASE/conversations?mailbox=<id>&status=<active|pending|closed>&modifiedSince=<ISO8601>&sortField=modifiedAt&sortOrder=desc`
- One conversation with its threads:
  `GET $BASE/conversations/<id>?embed=threads`
- Page with `&page=N`; walk `page.totalPages`.

Each conversation has `primaryCustomer` (with `email`), `status`,
`createdAt`, `customerWaitingSince`, and `tags`. The customer's email
domain is how you match an account in the CRM. Thread bodies are HTML —
strip it before reading, and read full threads only for conversations
your routine needs to judge.

Link for humans: `https://secure.helpscout.net/conversation/<id>/<number>`.

## Ground rules

Customer text stays in the support tool. Knowledge and CRM notes get
the account, the contact's name and role, a one-line paraphrase of what
they're waiting on, and the link — never quoted messages, never
personal details.

The API reference is at https://developer.helpscout.com/mailbox-api/ —
read the page when unsure of a parameter rather than guessing.
