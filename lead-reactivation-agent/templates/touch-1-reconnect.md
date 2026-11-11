# Warm Reconnect — Touch 1 (Touch Count 0)

Use for a lead who went quiet 30+ days ago. The goal is a reply, not a meeting.

**Subject pattern:** `Quick check-in on {{company}}`

```text
Hi {{first_name}},

You mentioned {{interest_summary}} a while back — is that still on your radar? If it is, I can share what has changed since we last spoke; if not, no problem at all.{{booking_sentence}}

Best,
{{sender_name}}
```

Fill rules:

- `{{interest_summary}}` — a short, faithful paraphrase of `Interest Notes`. Never invent a reason or a metric.
- `{{booking_sentence}}` — ` Happy to grab 15 minutes here: {{booking_link}}.` when a booking link exists, otherwise empty.
- Three sentences maximum, including the optional booking line.
- No "just circling back", no "hope you're well", no fake urgency.
- If the interest notes are empty, ask the user before writing — do not send a generic check-in.
