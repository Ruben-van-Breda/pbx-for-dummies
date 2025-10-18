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

## Common Pitfalls
- Wrong capture interface or filter: Missed packets; verify interface and use broader filters initially.
- SRTP media: RTP undecodable; rely on RTCP, ports, and packet rates instead of payload decode.
- Too short captures: Intermittent issues missed; capture long enough to see failure conditions.
- Low verbosity/log rotation: CLI output insufficient or lost; raise verbosity and persist logs.
- Time sync: Unaligned clocks across PBX/phones makes correlation hard; enable NTP.

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

---

## ✅ Quiz — Day 13 (10 Questions + Answers)

1) Which CLI commands enable useful SIP debug in Asterisk?
   - Answer: `pjsip set logger on` and `core set verbose 5`.

2) Where would you look first for one-way audio?
   - Answer: NAT/firewall and SDP/RTP address/port settings.

3) What Wireshark view shows call flows end-to-end?
   - Answer: Telephony → VoIP Calls → Flow Sequence.

4) What are CDRs used for?
   - Answer: Call Detail Records used for accounting and troubleshooting.

5) Why are short captures risky for intermittent issues?
   - Answer: They may miss the failure condition; capture long enough.

6) How do SRTP calls appear in Wireshark?
   - Answer: RTP payload is undecodable; rely on RTCP, ports, and timing.

7) What response classes indicate client vs server errors in SIP?
   - Answer: 4xx client errors; 5xx server errors.

8) Name one cause of missing packets in captures.
   - Answer: Wrong interface/filter; capture on the correct NIC and broaden filters.

9) Why is NTP important for troubleshooting?
   - Answer: Time sync across systems allows accurate correlation of logs and packets.

10) What should a troubleshooting log include?
   - Answer: Symptoms, hypotheses, tests, evidence (SIP/RTCP/CLI), root cause, and fix.
