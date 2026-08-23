<img src="avatar.svg" alt="" width="96" align="right">

An account assistant for the people who own customer relationships —
founders doing the selling, account managers, customer success — built
on [OpenRoutines](https://openroutines.dev). It keeps the picture of
every account current so you show up prepared: who's due a touch and
what's changed since you last talked, whose renewal is on the horizon
and whether it's an expansion or a rescue, whose health moved, who's
waiting on support, and what customers said in your shared channels
while you weren't looking.

It is consultative, never transactional. It prepares, drafts, and
flags; it never contacts a customer, never sends on anyone's behalf,
never promises anything. Everything it does is visible where you
already work: notes and tasks on your CRM records, a short brief in
Slack each weekday morning, and a daily check-in in
[Steady](https://runsteady.com) where the agent reports on itself as a
teammate.

## The routines

| Routine | What it does |
|---|---|
| upcoming-touches | Each weekday, finds the touches due in the next few days — CRM tasks and list entries — and prepares each one: what's changed since the last conversation, what the customer is waiting on, a suggested angle. Overdue touches become asks. |
| renewals-and-expansion | Weekly, walks the accounts renewing inside the horizon. Reads each one's health, usage, support, and channel record and calls it: expansion candidate, routine renewal, or at risk — with the evidence and talking points, as a CRM note. |
| health-shifts | Reads your customer health report and compares it against what it saw last time. Only material moves become events; a sharp drop near a renewal becomes an ask the same day. |
| support-watch | Watches the support inbox for named accounts: replies overdue, escalation language, churn signals. It flags; it never replies to a customer. |
| customer-channels | Reads your shared customer channels and Slack Connect DMs: unanswered questions, promises made, issues reported without a confirmed fix. One digest per account as a CRM note. |
| account-brief | Each weekday morning, one short Slack message: today's touches, renewals that moved, health that moved, support that needs eyes, what was heard in channels. Quiet day, no post. |
| steady-check-in | The agent's own daily check-in, filed in Steady as a teammate: what it did, what it will do, where it needs a human. |
| steady-inbox | Answers comments addressed to the agent in Steady and turns action requests into tracked tasks. Ships inactive. |

Each routine states its own boundary between what it does and what it
flags. The agent writes notes and tasks on CRM records and posts to
internal channels; anything customer-facing — an email, a reply, a
pricing conversation — is a human's, and the routine hands it over as
a drafted ask. Read any file in `routines/` to see exactly what it may
touch.

## Take it for a spin

Every routine has a rehearsal scenario in `rehearsals/` — one
consistent fictional vendor (Relay, a shared team inbox) and its
accounts, with a week of touches, a renewal that's quietly gone cold, a
health drop, an escalating support thread, and a channel where a
customer asked twice. A fixtured rehearsal strips all credentials and
never writes anything, so this works before any setup beyond the CLI
and Docker:

```bash
openroutines routines run upcoming-touches --rehearse
openroutines routines run renewals-and-expansion --rehearse
openroutines routines run account-brief --rehearse
```

Each prints exactly what it would have done — the prep it would hand
you, the call it would make on a renewal, the brief it would post. Edit
a prompt, rehearse again, watch the judgment change. That's the
[write–rehearse–run loop](https://openroutines.dev/docs/local-development/)
you'll use for routines of your own.

## Setup

You need the [OpenRoutines CLI](https://openroutines.dev/docs/getting-started/)
and about fifteen minutes.

1. **Use this template** to create your agent's repository, and clone it.
2. `openroutines configure` — fills in the owner, timezone, and model,
   and generates the `master.key` that encrypts credentials (back it up;
   it stays out of git).
3. Set `repo` in `openroutines.yml` to your new repository's URL.
4. **CRM.** The `crm` skill is written for Attio; it explains how to
   point it at HubSpot, Pipedrive, or whatever holds your accounts.
   Create an API key scoped to read records, lists, and tasks and write
   notes and tasks, then `openroutines credentials set crm_api_key`.
   Set the `crm_touches_list` and `crm_renewals_list` variables.
5. **Support desk.** The `support-desk` skill is written for Help Scout.
   Create a read-only app, put its App ID in `openroutines.yml`'s
   `support_desk_secret` entry, then
   `openroutines credentials set support_desk_secret` with the secret.
   To watch only some mailboxes, set `support_mailboxes` and add the
   same mailbox to `routines/support-watch.md`'s trigger URL.
6. **Team chat.** The `team-chat` skill reads your shared customer
   channels with a Slack *user* token (a bot can't see Slack Connect
   DMs): `openroutines credentials set chat_user_token`. Set
   `customer_channel_prefix` if yours isn't `c-`.
7. **Customer health.** Point `health_source` at your health report — a
   URL returning CSV or JSON, or a CRM attribute. The `customer-health`
   skill describes the shape. No health report yet? Leave it empty;
   health-shifts stays quiet and everything else works.
8. **Slack, for the brief.** Create a one-scope (`chat:write`) Slack app
   with a bot user, `openroutines credentials set slack_bot_token`,
   invite the bot to the channel, and put the channel ID in
   `brief_channel`.
9. **Steady, for the check-in.** Give the agent its own Steady account
   (Settings → Agents), then `openroutines credentials set steady_token`
   with its personal access token. Verify the wiring:
   `OPENROUTINES_LOG_LEVEL=warn openroutines routines run steady-verify`.
   See `.openroutines/plugins/steady/PLUGIN.md`.
10. `openroutines check`, commit, and
    [deploy](https://openroutines.dev/docs/deploying/).

This is your teammate now — rename it in `openroutines.yml`, retune the
schedules and thresholds, and edit the routine prompts like any other
file in your repo. Prefer the check-in somewhere else? Swap the
destination:
`openroutines plugin add steadyspacecorp/openroutines-plugins --path slack-report`
(or `--path discord-report`).

## Working on this agent

```bash
openroutines status                # what the agent has and still needs
openroutines sync                  # pull the latest knowledge; read the files under knowledge/
openroutines routines new <name>   # add a routine
openroutines routines run <name>   # real run; knowledge writes discarded (--write-knowledge settles)
openroutines check                 # validate everything; run it in CI
```

Deploying, updating, and everything else:
[OpenRoutines documentation](https://openroutines.dev).
