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

## Appendix — Deep Dives

### Deep Dive: SIP Error Taxonomy and Triage Flow

- What it is and why it matters: Classifying failures speeds resolution and avoids blind changes.
- Key details:
  - 4xx = client/request issues (auth, number format); 5xx = server/provider issues; 6xx = global.
  - Map common codes: 403 (forbidden), 404 (not found), 408 (timeout), 486 (busy), 488 (not acceptable), 500/503 (server/unavailable).
  - Triage: signaling succeeds but no media → inspect SDP, NAT, RTP ports.
- Practical checklist:
  - Start with CLI and precise codes; correlate timestamps with pcap and CDR.
  - Change one variable at a time; document each step.
- References: [Asterisk CLI](https://wiki.asterisk.org/wiki/display/AST/Asterisk+CLI)

### Deep Dive: RTP/RTCP Analysis for One‑Way and Choppy Audio

- What it is and why it matters: RTCP and packet patterns reveal path issues even when SRTP hides payloads.
- Key details:
  - Look for symmetric flow, consistent SSRC/ports, and increasing sequence numbers.
  - High jitter/loss correlates with choppy audio; confirm DSCP/QoS and network path.
  - SRTP: rely on ports/RTCP stats when payload is encrypted.
- Practical checklist:
  - Use Wireshark VoIP/RTCP stats; confirm RTP both directions.
  - Validate firewall/NAT pinholes match SDP.
- References: [Wireshark VoIP Analysis](https://www.wireshark.org/docs/wsug_html_chunked/ChTel.html)

### Deep Dive: Logging Strategy and Reproducible Test Cases

- What it is and why it matters: Structured logs and reproducible steps make fixes durable and auditable.
- Key details:
  - Raise verbosity selectively; persist logs; rotate responsibly.
  - Capture environment (IPs, codecs, versions) with each test; keep fixtures.
  - Red/green tests: prove failure, apply change, prove success.
- Practical checklist:
  - Template a troubleshooting log; include timestamps and exact commands.
  - Store small pcaps alongside notes for future reference.
- References: [Asterisk CDRs](https://wiki.asterisk.org/wiki/display/AST/Call+Detail+Records)

---

## ✅ Quiz — Day 13 (Deep Dives, 10 Questions + Answers)

1) What does a 488 response usually indicate?
   - Answer: Not acceptable here (e.g., codec/SDP mismatch).

2) First step when signaling works but no audio?
   - Answer: Inspect SDP addresses/ports and NAT/firewall.

3) What reveals choppy audio causes in encrypted calls?
   - Answer: RTCP stats, loss/jitter patterns, and RTP directionality.

4) How should test changes be applied during triage?
   - Answer: Change one variable at a time and document.

5) Two artifacts to attach to a troubleshooting log?
   - Answer: CLI excerpts and pcap/RTCP stats.

6) What does 404 vs 486 imply?
   - Answer: 404 not found (routing/number), 486 busy (callee busy).

7) Which Wireshark view visualizes the call ladder?
   - Answer: Telephony → VoIP Calls → Flow Sequence.

8) Why store small pcaps with notes?
   - Answer: Reproducibility and future regression checks.

9) What DSCP/QoS check helps with jitter?
   - Answer: Confirm proper DSCP marking and priority queuing.

10) What correlates logs and captures across systems?
   - Answer: Accurate NTP time sync.

