# Worked Example

A fictional pipeline, one run, on **2026-11-12**. Settings: `Daily Send Limit` 20, `High-Value Threshold` 10000, `Cooldown Days` 14, `Max Touches` 3, sender **Alex Rivera**, booking link `https://cal.example.com/alex`.

## Input: `Leads` tab

| Lead ID | Name | Company | Email | Last Touch | Status | Interest Notes | Touch Count | Deal Value |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| L-101 | Maya Chen | Blue Harbor Studio | maya@blueharbor.example | 2026-10-08 | Open | wanted a content site refresh before Q4 | 0 | 4800 |
| L-102 | Daniel Okafor | Northline Freight | daniel@northline.example | 2026-09-30 | Open | evaluating route-planning tools for 40 drivers | 0 | 18000 |
| L-103 | Sofia Ramos | Meridian Interiors | sofia@meridian.example | 2026-08-20 | Open | needed a client portal for project handoffs | 1 | 6200 |
| L-104 | Ace Facilities | Ace Facilities | ops@acefacilities.example | 2026-07-10 | Open | asked about monthly maintenance reporting | 0 | 2400 |
| L-105 | Priya Nair | Kestrel Labs | priya@kestrel.example | 2026-06-01 | Open | wanted lab inventory tracking | 2 | 7500 |
| L-106 | Jonas Feld | Feld & Co | jonas@feldco.example | 2026-05-15 | Open | exploring a quote tool for custom furniture | 0 | 9000 |
| L-107 | Ines Duarte | Lumen | ines@lumen.example | 2026-02-01 | Open | interested in a booking flow | 3 | 3000 |
| L-108 | Theo Marsh | Cobalt | theo@cobalt.example | 2026-10-30 | Open | asked about reporting dashboards | 0 | 5000 |
| L-109 | Hana Sato | Sato Design | hana@satodesign.example | 2026-03-10 | Unsubscribed | was comparing tools | 1 | 4000 |
| L-110 | Ben Yildiz | Yildiz Co | | 2026-06-20 | Open | asked for pricing | 0 | 3500 |

## Decisions the policy produces

| Lead | Days | Segment | Touch | Value | Decision | Why |
| --- | --- | --- | --- | --- | --- | --- |
| L-101 | 35 | Warm | 1 | $4,800 | **Send** | Warm, below threshold, touch 1 |
| L-102 | 43 | Warm | 1 | $18,000 | **Queue** | Above the high-value threshold |
| L-103 | 84 | Warm | 2 | $6,200 | **Send** | Warm, below threshold, touch 2 |
| L-104 | 125 | Cooling | 1 | $2,400 | **Send** | Cooling, below threshold, touch 1 |
| L-105 | 164 | Cooling | 3 | $7,500 | **Queue** | Close-file is always queued |
| L-106 | 181 | Cold | 1 | $9,000 | **Queue** | Cold segment |
| L-107 | 284 | Cold | — | $3,000 | **Skip** | Max touches reached — close-file candidate |
| L-108 | 13 | Not dormant | — | $5,000 | **Skip** | Inside the 30-day dormancy window |
| L-109 | 247 | — | — | $4,000 | **Skip** | Unsubscribed |
| L-110 | 145 | Cooling | — | $3,500 | **Skip** | No valid email |

## Rendered email for L-101

```text
Subject: Quick check-in on Blue Harbor Studio

Hi Maya,

You mentioned wanting a content site refresh before Q4 — is that still on your radar? If it is, I can share what has changed since we last spoke; if not, no problem at all. Happy to grab 15 minutes here: https://cal.example.com/alex.

Best,
Alex Rivera
```

Three sentences, one ask, only supplied facts.

## Run report

| Lead | Company | Segment | Touch | Value | Action |
| --- | --- | --- | --- | --- | --- |
| L-101 | Blue Harbor Studio | Warm | 1 | $4,800 | sent |
| L-103 | Meridian Interiors | Warm | 2 | $6,200 | sent |
| L-104 | Ace Facilities | Cooling | 1 | $2,400 | sent |
| L-102 | Northline Freight | Warm | 1 | $18,000 | queued |
| L-105 | Kestrel Labs | Cooling | 3 | $7,500 | queued |
| L-106 | Feld & Co | Cold | 1 | $9,000 | queued |
| L-107 | Lumen | Cold | — | $3,000 | skipped (max touches) |
| L-108 | Cobalt | Not dormant | — | $5,000 | skipped (under 30 days) |
| L-109 | Sato Design | — | — | $4,000 | skipped (unsubscribed) |
| L-110 | Yildiz Co | Cooling | — | $3,500 | skipped (no email) |

```text
Sent: 3 · Queued: 3 · Skipped: 4 · Failed: 0
```

Close-file candidates: **L-107 (Lumen)** — three touches, no reply. Recommend closing or archiving.

Broken rows to fix: **L-110** (missing email).
