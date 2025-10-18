# Day 13 — Monitoring & Troubleshooting

## Objectives
- Analyze SIP/RTP traffic
- Generate and read CDRs

## Concepts (Explained)
- SIP logging: Inspect signaling to pinpoint failures (4xx/5xx)
- RTP inspection: Jitter/loss analysis and one-way audio diagnosis
- CDRs: Call Detail Records for accounting and debugging

## Steps (15–20 min)
1) In Asterisk CLI:
```
pjsip set logger on
core set verbose 5
```
2) Make a test call; note responses and any errors
3) In Wireshark: Telephony → VoIP Calls → Flow Sequence
4) Review CDR output (CSV/db depending on config)

## Quick Check (Self-test)
- Where would you look first for one-way audio?
- Which SIP response classes indicate client vs server errors?

## Deliverable
- A short troubleshooting log with one example issue and resolution.
