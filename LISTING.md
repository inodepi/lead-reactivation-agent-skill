# Agensi Listing — Lead Reactivation Agent

Submission copy for https://www.agensi.io/dashboard/submit. Keep in sync with SKILL.md when behaviour changes.

## Fields

| Field | Value |
| --- | --- |
| Skill name (internal) | `lead-reactivation-agent` |
| Display title | Lead Reactivation Agent |
| Category | Sales & Lead Gen |
| Tagline | Revive the leads you already paid for — without sounding like a campaign. |
| Price | Free at launch — flip to $9 after the first 5 reviews |
| Version | 1.0.0 |
| License | MIT |
| Skills directory | `lead-reactivation-agent/` |

## Short description

Revives dormant sales leads end to end: reads your lead tracker, ranks leads by recency and value, writes a three-sentence personalized check-in, sends the safe touches automatically, and queues high-value or close-file emails for approval. Opt-outs are absolute and every touch is logged.

## Long description

Most pipelines don't die from a "no". They die from silence — the lead who said "not right now" fourteen months ago, still sitting in a list nobody works.

This skill turns your agent into the operator who works that list properly:

- **Finds the right leads.** Anyone touched in the last 30 days is skipped, so you never interrupt an active conversation. Opted-out, closed, duplicate, and broken rows are excluded and reported.
- **Segments by memory.** Warm (30–90 days), cooling (91–180), cold (181–365), dormant (365+). Recency decides the approach.
- **Runs a three-touch sequence.** Warm reconnect, a concrete value nudge, then a graceful "should I close your file?" — never a fourth chase.
- **Sends only what's safe.** Warm or cooling leads under your value threshold go out automatically. High-value accounts, cold and dormant leads, and every close-file email are queued for one yes/no.
- **Writes like a person.** Three sentences or fewer, references what the lead actually cared about, one low-friction ask, no invented links or claims.
- **Logs and reports.** Touch date and count go back to your tracker; you get sent, queued, skipped, failed, plus a list of leads ready to close.

Works with a spreadsheet, CSV, or CRM your agent can reach. Missing a connector? It tells you what to add instead of faking a send.

Includes three ready templates, the full qualification and cadence policy, a worked example, and — as a bonus — the original n8n workflow for teams that prefer draft-only automation.

## What's included

- `SKILL.md` — the end-to-end run method
- `references/qualification-and-cadence.md` — segments, ladder, compliance, edge cases
- `templates/` — reconnect, value nudge, close-file
- `examples/worked-example.md` — full sample run with decisions and report
- `assets/dead-lead-retrieval.n8n.json` — the companion n8n workflow

## Requirements

- Lead store read/write via connector, CRM, or CSV
- Email send via connector or email API
- A tracker with the standard headers (the skill maps them)

## Keywords

lead reactivation, win-back, dormant leads, dead leads, re-engagement, sales follow-up, pipeline, CRM cleanup, cold leads, outreach

## Security notes

- No credentials or API keys in the package
- Requires email-send capability, declared on the listing
- Opt-outs are absolute; high-value and close-file emails always need approval
- Example data uses fictional `.example` addresses only

## Promotion

- Link from the companion repo README and the portfolio film
- Pair with the AR Collections Agent as a "revenue operations" bundle later
- Post the worked example as a thread: before/after of a stale list
