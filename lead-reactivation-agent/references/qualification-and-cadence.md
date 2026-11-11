# Qualification and Cadence

The reasoning behind the skill's fixed rules. Read this when a lead is ambiguous, the user asks why someone was skipped, or they want to tune the campaign.

## Why recency leads

A lead goes cold for one of three reasons: the timing was wrong, the budget moved, or they chose someone else. All three come back around. Recency is the best available proxy for whether the memory of you is still alive — a lead from six weeks ago remembers the conversation; a lead from two years ago needs a reintroduction, not a follow-up.

| Days since last touch | Segment | What it means | Default treatment |
| --- | --- | --- | --- |
| under 30 | Not dormant | They are still in the active pipeline | Skip — let the normal follow-up run |
| 30–90 | Warm | They remember you and the problem is probably still open | Auto-send allowed |
| 91–180 | Cooling | Context is fading; needs a reason to re-engage | Auto-send allowed |
| 181–365 | Cold | Needs a fresh angle, and a human should see it | Queue |
| 365+ | Dormant | A reintroduction, not a follow-up | Queue |

Within a segment, higher deal value first — with one exception: a high-value lead is *more* sensitive, which is why value above the threshold routes to approval.

## The three-touch ladder

| Touch | Template | What it does | Auto-send? |
| --- | --- | --- | --- |
| 0 → 1 | Warm reconnect | References what they originally cared about, asks one easy question | Yes, Warm/Cooling, below value threshold |
| 1 → 2 | Value nudge | Offers something new — an idea, a concrete next step | Yes, same conditions |
| 2 → 3 | Close the file | Graceful exit; invites them back later | Never — always queued |

After touch 3 the lead is done. `Max Touches` exists so a quiet lead is closed politely rather than chased forever; that protects the sender's domain reputation as much as the relationship.

Recommended spacing is one touch per `Cooldown Days` (default 14), so a full sequence runs about six weeks. Running the skill more often than the cooldown does nothing — the skip rule handles it.

## Send policy

| Condition | Action |
| --- | --- |
| Warm or Cooling, value below threshold, touch 1–2 | Send |
| Cold or Dormant | Queue |
| Value at or above threshold | Queue |
| Touch 3 | Queue |
| Anything the user flagged as sensitive | Queue |
| First run | Full review, then autonomous |

The asymmetry is deliberate. A missed low-value nudge costs a few days of pipeline. An unprompted email to a $50k account, or a "closing your file" note sent by mistake, costs the relationship and the sale.

## Compliance and trust

- **Opt-outs are absolute.** `Opt-Out`, unsubscribe, "remove me", "not interested", "do not contact" — skip the lead and never write a workaround. Report them as excluded.
- **No invented claims.** No fake case studies, no invented metrics, no guessed URLs. The only link allowed is the user's own booking link.
- **One next step per email.** A reply question or the booking link, not both stacked with a second ask.
- **Quiet leads get closed.** Touch 3 exists so the sequence ends with dignity instead of silence.
- **B2B norms.** These are people the user has already spoken to, not a purchased list. The skill is not a prospecting or scraping tool, and the description says so.
- **Records.** Every touch is written back to the tracker, so the user can show who was contacted, when, and why.

## Risk signals that force a queue

- deal value at or above the high-value threshold
- Cold or Dormant segment
- touch 3 (close-file)
- a lead the user flagged as sensitive or VIP
- a thread that already contains a "not now" answer
- any ambiguity about who the contact is (role change, shared inbox)

## Edge cases

**No interest notes.** Do not invent a reason. Use the company and the last conversation date if the user can supply it; otherwise ask before drafting. A generic "checking in" email fails the whole point.

**Role changed or bounce.** Skip and report. Do not guess a successor's address; offer to find the new contact instead.

**Several contacts at one company.** One email per company per run. Two colleagues receiving the same nudge looks automated.

**Missing deal value.** Treat as below threshold for sending, but list it in the report so the user can fill it in. Do not invent a number.

**Positive reply during a run.** Stop the sequence for that lead, mark it in the report, and leave `Touch Count` alone — the user handles the conversation.

**Shared inboxes** (`info@`, `sales@`). Queue rather than auto-send; a human should pick the right recipient.

**Max touches reached.** Report as a close-file candidate. The skill does not email them again; closing or archiving is the user's decision.

**Deliverability.** Respect `Daily Send Limit`. If the user reports bounces or spam complaints, pause the campaign and suggest warming the domain or thinning the list.

**Status vocabulary.** CRM statuses differ (`TO DO`, `Draft Done`, `Follow-up Sent` in the source workflow). Map the user's statuses once, confirm the mapping, and never write a status the CRM does not have.

## Reporting

Per-lead rows, segment and touch shown, skip reasons named. Close-file candidates listed separately. No silent failures.
