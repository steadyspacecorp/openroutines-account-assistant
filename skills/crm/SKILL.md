---
name: crm
description: Read accounts, lists, and tasks from the CRM and write notes and tasks on account records. Written for Attio; adapt the endpoints for HubSpot, Pipedrive, or whatever holds your accounts.
---

# CRM

The CRM is the system of record for accounts: who they are, who owns
them, what's scheduled, when they renew. Routines read it to know what's
due and write back only two things — **notes** (the agent's prep and
digests, on the account's record) and **tasks** (asks for the humans who
own the relationship). Nothing else on a record is yours: never change a
stage, an owner, a date, a deal value, or a contact.

Authentication is ambient: `$CRM_API_KEY` arrives in runs that declare
the credential. Never print, inspect, or write it anywhere.

## Attio (worked example — replace with your CRM)

```bash
AUTH="Authorization: Bearer $CRM_API_KEY"
BASE=https://api.attio.com/v2
```

**Lists and their entries** — `$CRM_TOUCHES_LIST` and
`$CRM_RENEWALS_LIST` are list slugs or ids:

```bash
curl -s -H "$AUTH" "$BASE/lists"                                  # discover lists and slugs
curl -s -H "$AUTH" "$BASE/lists/$CRM_RENEWALS_LIST/attributes"    # the list's own attributes
curl -s -H "$AUTH" -H "Content-Type: application/json" \
  -X POST "$BASE/lists/$CRM_RENEWALS_LIST/entries/query" \
  -d '{"limit": 100}'
```

Entries carry `parent_record_id` (the account) and `entry_values` keyed
by the list's attribute slugs — a renewal date, a stage, an expansion
flag, a next-touch date live there, under whatever names the team chose.
Discover the attribute slugs on your first run and record the mapping in
your ledger; never guess a slug twice. Page with `offset`.

**Tasks** — the team's scheduled touches and your own asks:

```bash
curl -s -H "$AUTH" "$BASE/tasks?is_completed=false&limit=100"
curl -s -H "$AUTH" "$BASE/tasks?linked_object=companies&linked_record_id=RECORD_ID&is_completed=false"
```

A task has `content`, `deadline_at`, `assignees`, and `linked_records`.
To raise an ask on an account:

```bash
curl -s -H "$AUTH" -H "Content-Type: application/json" -X POST "$BASE/tasks" \
  -d '{"data": {"content": "…", "format": "plaintext", "deadline_at": "2026-08-28T16:00:00Z",
       "is_completed": false,
       "linked_records": [{"target_object": "companies", "target_record_id": "RECORD_ID"}],
       "assignees": [{"referenced_actor_type": "workspace-member", "referenced_actor_id": "OWNER_MEMBER_ID"}]}}'
```

Assign to the account's owner (a workspace member on the record; list
them with `GET $BASE/workspace_members`). Never complete or edit a
human's task.

**Accounts** — find by domain first, name second:

```bash
curl -s -H "$AUTH" -H "Content-Type: application/json" \
  -X POST "$BASE/objects/companies/records/query" \
  -d '{"filter": {"domains": "example.com"}, "limit": 3}'
curl -s -H "$AUTH" "$BASE/objects/companies/records/RECORD_ID"
```

The record id is `data[].id.record_id`. If no record matches, do not
create one — report the orphan in your run output.

**Notes** — the account's recent history, and where your prep lands:

```bash
curl -s -H "$AUTH" "$BASE/notes?parent_object=companies&parent_record_id=RECORD_ID&limit=10"
curl -s -H "$AUTH" -H "Content-Type: application/json" -X POST "$BASE/notes" \
  -d '{"data": {"parent_object": "companies", "parent_record_id": "RECORD_ID",
       "title": "Touch prep 2026-08-24", "format": "plaintext", "content": "…\n\nrun: RUN_ID"}}'
```

Stamp `$OPENROUTINES_RUN_ID` on the last line of every note you write and
list the record's recent notes before writing: a retried run finds its
own note instead of writing a second one.

When unsure of a query parameter or response shape, read the reference
at https://docs.attio.com/rest-api rather than guessing.

## Adapting to another CRM

Keep the contract, swap the calls: a way to list the accounts on the two
lists with their dates, a way to read open tasks and raise one, a way to
find an account by domain and read its recent notes, a way to write a
note. HubSpot has lists, tasks (engagements), and notes; Pipedrive has
filters, activities, and notes. The write surface stays notes and tasks
only.
