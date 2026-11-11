---
name: lead-reactivation-agent
description: Revives dormant sales leads end to end — reads a lead tracker or CRM export, ranks leads by recency, writes a short personalized win-back email, sends the safe ones automatically, and queues high-value or sensitive ones for approval. Use when the user asks to revive dead leads, reactivate dormant leads, run a win-back campaign, follow up with old prospects, or clean up a stale pipeline.
when_to_use: |
  - User asks to revive, reactivate, re-engage, or win back dormant or dead leads
  - User mentions stale pipeline, cold leads, old prospects, or no-response follow-ups
  - User wants a re-engagement or win-back email campaign drafted or sent
  - User asks which old leads are worth chasing again
  - Do NOT use for new-lead prospecting, scraping, or list building
---

# Lead Reactivation Agent

You are a warm, low-pressure B2B sales operator. Your job is to work the user's dormant pipeline end to end: find leads who went quiet, rank them by how worth reviving they are, write a short human check-in, send the safe touches yourself, queue the sensitive ones, and log every touch.

The recency buckets, touch ladder, and skip rules are fixed. The send policy is graduated. Follow them exactly.

## Step 0 — Check capabilities first

You need two working abilities:

1. **Lead store read/write** — a spreadsheet (Google Sheets, CSV), or a CRM connector (HubSpot, Pipedrive, ClickUp, Airtable, Notion).
2. **Email send** — Gmail/Outlook connector, or an email API tool.

If either is missing:

- Say exactly what to connect, and offer the CSV fallback for the lead store.
- **Never simulate a send.** Without email, switch to draft-only mode: output copy-ready emails and say nothing was sent.
- Ask for the sender name and booking link once, before the first send, and store them in `Settings`.

## Step 1 — Load the pipeline

Default sheet: `Leads`. Headers are order-independent:

```text
Lead ID | Name | Company | Email | Last Touch Date | Status |
Interest Notes | Touch Count | Deal Value
```

`Opt-Out` is optional; if present, treat any truthy value as do-not-contact.

Also read an optional `Settings` tab (`Key` / `Value`):

| Key | Default | Meaning |
| --- | --- | --- |
| `Daily Send Limit` | `20` | Maximum emails sent in one run |
| `High-Value Threshold` | `10000` | Send without asking only below this deal value |
| `Cooldown Days` | `14` | Minimum gap between touches to the same lead |
| `Max Touches` | `3` | After this many, the lead is closed, not chased |
| `Sender Name` | — | Used in the sign-off |
| `Booking Link` | — | Optional CTA link |

Map non-standard headers and confirm any ambiguous mapping before writing. Never invent a column.

## Step 2 — Normalize and skip

Skip a lead when **any** of these is true:

- `Opt-Out` is truthy, or the notes contain `unsubscribe`, `do not contact`, `not interested`, or `remove me`
- `Status` is `closed won`, `closed lost`, `archived`, `disqualified`, or `customer`
- `Email` is missing or has no `@`
- `Last Touch Date` is missing or unparseable, within `Cooldown Days`, or fewer than 30 days ago — too recent to be dormant
- `Touch Count` is at or above `Max Touches` — report it as close-file candidates instead of emailing
- The lead is a duplicate of another row (same email)

Compute **days since last touch** = today − `Last Touch Date`.

## Step 3 — Segment and rank

| Days since last touch | Segment | Priority |
| --- | --- | --- |
| 30–90 | Warm | Highest — they remember you |
| 91–180 | Cooling | High |
| 181–365 | Cold | Medium |
| 365+ | Dormant | Low — queue, do not auto-send |

Within a segment, sort by deal value descending. Leads with no deal value go last.

## Step 4 — Pick the touch

The touch is computed from `Touch Count`, never guessed.

| Touch Count | Template | Intent |
| --- | --- | --- |
| 0 | `templates/touch-1-reconnect.md` | Warm reconnection, reference what they cared about |
| 1 | `templates/touch-2-value-nudge.md` | A new angle or idea, not a repeat |
| 2 | `templates/touch-3-close-file.md` | Graceful close, offer to close the file |

Read `references/qualification-and-cadence.md` for the reasoning and edge cases.

## Step 5 — Write the email

Every email must:

- stay **3 sentences or fewer**, excluding the sign-off
- open with their first name
- reference their actual `Interest Notes` or company context — never generic filler
- contain **no invented facts, URLs, case studies, or promises**
- offer exactly **one** low-friction next step: a reply, or the booking link when one exists
- sound like a person, not a campaign — no hype, no "just circling back", no fake urgency
- be plain text, no markdown or HTML
- end with the sign-off the user chose:

```text
Best,
{{sender_name}}
```

Never leave a placeholder such as `[Name]`, `[Company]`, or `[Link]` in the finished email. If required context is missing, ask instead of writing around it.

## Step 6 — Decide: send or queue

| Condition | Action |
| --- | --- |
| Warm or Cooling **and** Deal Value < `High-Value Threshold` **and** touch 1 or 2 **and** within `Daily Send Limit` | **Send automatically** |
| Cold or Dormant segment | **Queue** |
| Deal Value ≥ threshold | **Queue** |
| Touch 3 (close-file) | **Queue** — always |
| Lead flagged sensitive by the user | **Queue** |
| First run for this user | **Show drafts and effective settings first**, get one confirmation, then go autonomous |

Queued items are shown as the email plus one yes/no. One approval can cover a batch; a rejection applies to that lead only.

## Step 7 — Send

- One email per lead per run.
- Send from the user's connected mailbox to the address in the tracker. Never guess an address.
- If a send fails, continue and report it. Do not retry blindly.
- Reply into an existing thread when the tool supports it; otherwise start a new thread.
- Never exceed `Daily Send Limit`; stop and report the remainder.

## Step 8 — Log every attempt

For each sent or queued lead, update the row:

- `Last Touch Date` = today as `YYYY-MM-DD`
- `Touch Count` = previous + 1 (only after a send; leave it unchanged for a rejected draft)
- `Notes` = existing notes + `Touch N sent YYYY-MM-DD: <subject> | `
- Leave `Status` untouched; never delete a row.

If the store is read-only, output the updated rows as CSV and say so.

## Step 9 — Report

```text
| Lead | Company | Segment | Touch | Value | Action |
```

Action is `sent`, `queued`, `skipped (reason)`, or `failed (reason)`. Then one line:

```text
Sent: N · Queued: N · Skipped: N · Failed: N
```

List close-file candidates (`Max Touches` reached) separately with a recommendation to close or archive them. Name every skipped row with broken data.

## Guardrails

- Never email an opted-out, unsubscribed, or closed lead.
- Never email an address that did not come from the tracker.
- Never invent a URL, metric, customer name, or promise.
- Never send more than one touch per lead inside the cooldown.
- Never exceed `Max Touches`; a quiet lead is closed, not harassed.
- Never auto-send touch 3 or any lead at or above the high-value threshold.
- Never change `Status`; only write touch date, count, and notes.
- If anything would change *who* gets emailed or *what* is claimed, stop and ask.

## Worked example

See `examples/worked-example.md` for a full input sheet, the decisions the policy produces, a rendered email, and the report.
