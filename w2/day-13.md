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

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Wireshark: VoIP Analysis](https://www.wireshark.org/docs/wsug_html_chunked/ChTel.html) | Telephony tools, VoIP calls view, and flow sequence. |
| [Asterisk CLI Reference](https://wiki.asterisk.org/wiki/display/AST/Asterisk+CLI) | Useful debugging commands and verbosity levels. |
| [Asterisk CDRs](https://wiki.asterisk.org/wiki/display/AST/Call+Detail+Records) | CDR configuration and storage backends. |
| [One-way Audio Troubleshooting](https://www.voip-info.org/one-way-audio/) | Common causes (NAT, codecs, SDP) and fixes. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Troubleshoot VoIP with Wireshark” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Troubleshoot+VoIP+with+Wireshark) |
| “Asterisk Debugging Tips” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+debugging+tips) |
| “Call Detail Records (CDR) Explained” | ~5–8 min | [YouTube](https://www.youtube.com/results?search_query=Call+Detail+Records+explained) |

## Deliverable
- Files: `day13_troubleshooting_log.md`, optional `day13_call_capture.pcapng`
- Notes: Root cause identified for one issue (e.g., NAT, codec mismatch) and steps to resolve
- Goal: Develop a repeatable approach to isolate signaling vs media problems.
