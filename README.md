![Lead Reactivation Agent](assets/logo-512.png)

# Lead Reactivation Agent

A SKILL.md skill that turns your AI agent into a low-pressure win-back operator. Point it at your stale pipeline and it finds the leads worth reviving, writes a short human check-in, sends the safe touches itself, and queues only the emails that deserve your review.

Built from the [Dead Lead Retrieval](https://github.com/potaterrr/dead-lead-retrieval) n8n workflow — same warm three-sentence voice, same no-invented-URLs rule, but the agent sends instead of filing drafts.

## What it does

1. Checks it actually has lead-store and email access — stops and tells you what to connect if not
2. Reads your lead tracker and computes days since last touch
3. Skips opted-out, closed, duplicate, broken, and too-recent leads
4. Segments leads: warm, cooling, cold, dormant
5. Picks the right touch — reconnect, value nudge, or close-file
6. Sends warm/cooling touches under your value threshold automatically
7. Queues high-value, cold, dormant, and every close-file email for approval
8. Logs touch date and count back to the tracker
9. Reports sent, queued, skipped, failed — plus close-file candidates

## Graduated autonomy

| Situation | What happens |
| --- | --- |
| Warm or cooling, under $10k, touch 1–2 | Sent automatically |
| Cold or dormant | Drafted, then one yes/no from you |
| Deal value at or above $10k | Drafted, then one yes/no |
| Touch 3 — "close your file?" | Always requires approval |
| First ever run | Full review, then hands-off |

Everything is configurable in an optional `Settings` tab: daily send limit, high-value threshold, cooldown, max touches, sender name, booking link.

## The three-touch ladder

| Touch | Email | Asks for |
| --- | --- | --- |
| 1 | Warm reconnect | A simple "yes, still relevant" reply |
| 2 | Value nudge | Permission to send one concrete idea |
| 3 | Close the file | A reply now, or a clean close with an open door later |

Every email is three sentences or fewer, references the lead's actual interest notes, uses no invented facts or links, and ends with your chosen sign-off.

## What's inside

```text
lead-reactivation-agent/
├── SKILL.md                          # the run method the agent follows
├── references/
│   └── qualification-and-cadence.md  # segments, touch ladder, edge cases, compliance
├── templates/
│   ├── touch-1-reconnect.md
│   ├── touch-2-value-nudge.md
│   └── touch-3-close-file.md
├── examples/
│   └── worked-example.md             # sample pipeline, decisions, rendered email, report
└── assets/
    └── dead-lead-retrieval.n8n.json  # bonus: the draft-only n8n workflow
```

## Requirements

- A lead store your agent can read and write — Google Sheets connector, CRM connector, or a CSV
- An email tool it can send from — Gmail/Outlook connector or email API
- A tracker with these headers: `Lead ID`, `Name`, `Company`, `Email`, `Last Touch Date`, `Status`, `Interest Notes`, `Touch Count`, `Deal Value`, optional `Opt-Out`

No credentials live in this repo. Nothing is sent until your agent runs this skill.

## Get it

- **Agensi:** _listing link after publication_
- **Local:** copy the `lead-reactivation-agent/` folder into your agent's skills directory (for Claude Code: `~/.claude/skills/lead-reactivation-agent/`). The folder name must match `name` in the frontmatter.

## Prefer draft-only automation?

The `assets/` folder carries the original n8n workflow: a daily ClickUp sweep, LLM check-in drafting, and Gmail drafts that a human always sends. Import it into n8n and attach your own credentials.

Full workflow documentation lives in the [companion repo](https://github.com/potaterrr/dead-lead-retrieval).

## Safety

- Opt-outs are absolute; unsubscribed leads are never emailed
- One touch per lead per cooldown (14 days), three touches maximum
- High-value accounts always see a human first
- `Status` is never modified

## License

MIT
