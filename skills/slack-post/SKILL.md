---
name: slack-post
description: Post a message to a Slack channel with chat.postMessage -- payload shape, Block Kit formatting, the ok:true delivery check, and the rules that keep an unattended poster well-behaved. Use when a routine needs to send anything to Slack via $SLACK_BOT_TOKEN.
---

# Posting to Slack via chat.postMessage

The bot token arrives as `$SLACK_BOT_TOKEN` and the target channel ID as
`$BRIEF_CHANNEL`. Never print the token, never include it in a message.
The token's only scope is `chat:write`: it can post solely to channels
the bot has been invited to, and this skill posts solely to
`$BRIEF_CHANNEL`.

## Sending

Write scratch files under `$TMPDIR` (the run's writable tmp) -- the
sandbox makes `/tmp` itself read-only, so `/tmp/...` paths fail.

```bash
curl -sS -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @"$TMPDIR/payload.json" > "$TMPDIR/slack-resp"
```

Delivery is `"ok": true` in the response body -- **Slack returns HTTP 200
even for failures**, so never treat the status code as success. On
`"ok": false` read the `error` field; the common ones:

- `not_in_channel` -- the bot was never invited (or was removed); a
  person must `/invite` it. Raise this as a Human-owned task rather than
  retrying.
- `channel_not_found` -- `$BRIEF_CHANNEL` is wrong or the channel is
  gone; same treatment.
- `invalid_auth` / `token_revoked` -- the credential needs re-setting.

Do not retry more than once in a run.

## Payload shape

Always include the channel, a `text` fallback (used by notifications and
screen readers -- lead with the day's headline outcome, never a generic
label), then `blocks` for structure:

```json
{
  "channel": "$BRIEF_CHANNEL",
  "text": "Two touches today, Kestrel renewal at risk -- one ask for Dana",
  "blocks": [
    { "type": "header", "text": { "type": "plain_text", "text": "Account brief" } },
    { "type": "section", "text": { "type": "mrkdwn", "text": "*Today*\n• Brightline QBR 10:00 -- prep on the <https://example.com/notes/42|account note>" } },
    { "type": "section", "text": { "type": "mrkdwn", "text": "*Needs a human*\n• Kestrel renews 10-15 with no call booked -- Dana to schedule" } }
  ]
}
```

(Substitute the real channel ID when building the payload; JSON does not
expand `$BRIEF_CHANNEL`.)

Slack mrkdwn is not markdown: links are `<url|text>`, bold is `*text*`,
bullets are literal `•` characters. Keep any single section block under
3000 characters; split long lists across blocks.

## Conduct

- Never use `@channel`, `@here`, or user pings -- an unattended agent
  earns attention with content, not interruptions.
- One message per run, maximum. Batch, don't stream.
- No secrets, tokens, or internal URLs the channel's audience shouldn't
  see; when unsure, name the thing without linking it.
