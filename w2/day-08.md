# Day 8 — NAT Traversal: STUN, TURN, ICE

## Objectives
- Understand why VoIP fails behind NAT
- Learn how STUN, TURN, and ICE enable media connectivity

## Key Concepts (Explained)
- NAT problem: Private IPs are unroutable from the public internet; SIP/SDP often advertises private addresses.
- STUN: Endpoint discovers its public IP/port mapping.
- TURN: Relays media via a public server when peer-to-peer fails.
- ICE: Gathers candidates (host, reflexive, relayed) and tests paths to choose the best.

## Media Path Logic
1) Offer/answer exchange includes ICE candidates
2) Connectivity checks run in parallel (STUN Binding requests)
3) Best working candidate pair is selected; others pruned

## Hands-on (15–20 min)
1) Draw a quick diagram of host/reflexive/relayed candidates
2) If available, test a WebRTC call (e.g., `https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/`) and observe candidates
3) Note why PBX/SBCs often anchor media to simplify NAT

## Quick Check (Self-test)
- When is TURN required?
- What does STUN provide that SIP alone does not?
- Why can symmetric NATs be problematic?

## Common Pitfalls
- Missing/invalid STUN or TURN servers: Candidates never gather or fail; verify URLs and credentials.
- Symmetric NAT/firewall: Peer-to-peer fails; expect TURN relay (added latency/cost).
- ICE disabled or no trickle ICE: Slow negotiation or timeouts; enable trickle where possible.
- UDP/RTP ports blocked: SIP works but no audio; open provider RTP ranges and test.
- Wrong public IP in SDP: Use PBX/SBC external address settings or anchor media.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Interactive Connectivity Establishment (ICE) — WebRTC](https://webrtc.org/getting-started/ice) | Clear overview of ICE and candidate types. |
| [STUN & TURN — MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Connectivity) | How STUN/TURN are used in WebRTC connectivity. |
| [RFC 5389 — STUN](https://www.rfc-editor.org/rfc/rfc5389) | Session Traversal Utilities for NAT (STUN) spec. |
| [RFC 5766 — TURN](https://www.rfc-editor.org/rfc/rfc5766) | Traversal Using Relays around NAT spec. |
| [RFC 8445 — ICE](https://www.rfc-editor.org/rfc/rfc8445) | Updated ICE specification. |
| [Asterisk & NAT](https://wiki.asterisk.org/wiki/display/AST/Asterisk+SIP+NAT+support) | NAT support considerations for Asterisk deployments. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “STUN, TURN, ICE Explained” | ~7–12 min | [YouTube](https://www.youtube.com/results?search_query=STUN+TURN+ICE+explained) |
| “WebRTC ICE Candidates Tutorial” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=WebRTC+ICE+candidates+tutorial) |
| “NAT Traversal for VoIP” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=NAT+traversal+for+VoIP) |

## Deliverable
- File: `day8_ice_candidates.png` (diagram of host/reflexive/relayed) or annotated screenshot from Trickle ICE
- Notes: When TURN is needed, which candidate pair succeeded, and why
- Goal: Demonstrate understanding of NAT traversal strategies and when to anchor media.

---

## ✅ Quiz — Day 8 (10 Questions + Answers)

1) What problem does NAT introduce for VoIP media?
   - Answer: Private IP/ports are not reachable from the internet; SDP often advertises unroutable addresses.

2) What does STUN help an endpoint discover?
   - Answer: Its public IP and port mapping as seen by the STUN server.

3) When is TURN required?
   - Answer: When direct peer-to-peer connectivity fails (e.g., symmetric NAT/firewalls) and media must be relayed.

4) What does ICE do during call setup?
   - Answer: Gathers host/reflexive/relayed candidates and runs connectivity checks to select the best path.

5) Name the three main ICE candidate types.
   - Answer: Host, server-reflexive, and relayed.

6) Why can symmetric NATs be problematic for VoIP?
   - Answer: They map differently per destination, breaking incoming traffic unless relayed via TURN.

7) What is trickle ICE?
   - Answer: Incremental candidate gathering/signaling so checks start before all candidates are known.

8) One-way audio occurs though SIP succeeds. Give two likely causes.
   - Answer: RTP ports blocked by firewall or wrong public IP in SDP.

9) Why do PBX/SBCs often anchor media?
   - Answer: To hide/normalize endpoints behind NAT and ensure stable, routable RTP paths.

10) What tradeoff does TURN introduce?
   - Answer: Higher latency and relay bandwidth cost in exchange for reliable connectivity.

## Appendix — Deep Dives

### Deep Dive: ICE Candidate Nomination (Regular vs Aggressive)

- What it is and why it matters: Nomination is how the controlling agent chooses the final working candidate pair for each component. Picking too early can cause sub‑optimal paths; picking too late can delay media.
- Key details:
  - Regular nomination waits for the highest‑priority successful check and then marks it with USE‑CANDIDATE; this is the RECOMMENDED mode for interoperability and with trickle ICE. [RFC 8445 (ICE)](https://www.rfc-editor.org/rfc/rfc8445)
  - Aggressive nomination adds USE‑CANDIDATE on the first check and can prematurely lock in a non‑optimal path; avoid when using trickle ICE.
  - Role matters: exactly one side must be controlling. If both believe they are controlling, resolve the role conflict per tie‑breaker rules before continuing. [RFC 8445 §6](https://www.rfc-editor.org/rfc/rfc8445#section-6)
  - PBX/SBC note: when interacting with ICE‑lite endpoints, the full agent acts as controlling and performs nomination. [RFC 8445 §2.7](https://www.rfc-editor.org/rfc/rfc8445#section-2.7)
- Practical checklist:
  - Ensure only one controlling agent; watch for role‑conflict errors.
  - Prefer regular nomination; test with trickle and rollback flows.
  - Verify the nominated pair actually carries RTP/RTCP (not just STUN success).
- References: [RFC 8445 (2018)](https://www.rfc-editor.org/rfc/rfc8445), [WebRTC ICE Overview](https://webrtc.org/getting-started/ice)

### Deep Dive: TURN Allocations, Permissions, and Channels

- What it is and why it matters: TURN provides a publicly reachable relay address when direct connectivity fails, enabling media to traverse symmetric NATs and strict firewalls.
- Key details:
  - Allocation: client obtains a relayed transport address with a finite lifetime (default 10 minutes) and must refresh periodically. [RFC 5766 §7](https://www.rfc-editor.org/rfc/rfc5766#section-7)
  - Permissions: per‑peer IP allowlists required for UDP data; each permission lasts 300 seconds unless refreshed. [RFC 5766 §8](https://www.rfc-editor.org/rfc/rfc5766#section-8)
  - Channels: optional low‑overhead data path bound to a peer (16‑bit Channel Number); lifetime typically 10 minutes; improves throughput vs Send Indications. [RFC 5766 §11](https://www.rfc-editor.org/rfc/rfc5766#section-11)
  - Auth: TURN uses the STUN long‑term credential mechanism (username/realm/nonce with HMAC‑SHA1 integrity); servers return 401/438 for auth/nonce issues. [RFC 5389 §9](https://www.rfc-editor.org/rfc/rfc5389#section-9), [RFC 5766 §4](https://www.rfc-editor.org/rfc/rfc5766#section-4)
  - Transports/ports: UDP/TCP on 3478, TLS on 5349; DTLS and TCP/TLS commonly used for WebRTC. Ensure firewall egress to your TURN and provider IPs. [RFC 5766](https://www.rfc-editor.org/rfc/rfc5766)
- Practical checklist:
  - Verify credentials and nonce handling; watch for 401/438 and refresh tokens.
  - Monitor allocation refreshes; expired allocations silently break media.
  - Prefer channels for sustained media; ensure permissions are refreshed every ~4–5 minutes.
- References: [RFC 5766 (TURN, 2010)](https://www.rfc-editor.org/rfc/rfc5766), [RFC 5389 (STUN, 2008)](https://www.rfc-editor.org/rfc/rfc5389), [MDN — STUN/TURN](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Connectivity)

### Deep Dive: ICE Lite vs ICE Full (Servers, PBXs, SBCs)

- What it is and why it matters: ICE‑Lite is a simplified profile intended for servers on public IPs; it reduces complexity by not performing connectivity checks, while remaining interoperable with full ICE peers.
- Key details:
  - ICE‑Lite agents gather only host candidates and do not run connectivity checks; they indicate `ice-lite` in SDP. [RFC 8445 §2.7](https://www.rfc-editor.org/rfc/rfc8445#section-2.7)
  - When talking to ICE‑Lite, the full agent MUST be controlling and performs all checks and nomination. [RFC 8445 §6.1](https://www.rfc-editor.org/rfc/rfc8445#section-6.1)
  - Suitable for PBX/SBCs that anchor media on public addresses; not appropriate when the server itself is behind NAT or requires relayed candidates.
  - Interop: still compatible with trickle ICE—full agent trickles; lite agent responds.
- Practical checklist:
  - Use ICE‑Lite on publicly reachable media relays/SBCs to simplify NAT.
  - If any side is behind NAT or multi‑homed with policy routing, prefer full ICE.
  - Always include accurate public addresses in SDP and verify RTCP.
- References: [RFC 8445 (ICE, 2018)](https://www.rfc-editor.org/rfc/rfc8445), [WebRTC ICE Overview](https://webrtc.org/getting-started/ice)

---

## ✅ Quiz — Day 8 (Deep Dives, 10 Questions + Answers)

1) In regular nomination, when is USE‑CANDIDATE applied?
   - Answer: Only after the highest‑priority successful check is selected for nomination.

2) Why is aggressive nomination risky with trickle ICE?
   - Answer: It can nominate an early, sub‑optimal pair before higher‑priority candidates arrive.

3) Who performs connectivity checks when a full ICE agent talks to an ICE‑Lite agent?
   - Answer: The full ICE agent (it must be controlling).

4) What lifetime must a TURN client maintain to keep an allocation active by default?
   - Answer: About 10 minutes, refreshed periodically (per RFC 5766).

5) How long do TURN permissions last if not refreshed?
   - Answer: 300 seconds (5 minutes).

6) What advantage do TURN channels provide over Send Indications?
   - Answer: Lower overhead and better throughput once a channel is bound.

7) Which authentication mechanism does TURN use?
   - Answer: STUN long‑term credentials with HMAC‑SHA1 integrity (username/realm/nonce).

8) Which ports are commonly used for TURN?
   - Answer: 3478 (UDP/TCP) and 5349 (TLS).

9) What attribute signals an endpoint is ICE‑Lite?
   - Answer: The `ice-lite` attribute in SDP.

10) What resolves an ICE role conflict when both sides think they are controlling?
   - Answer: The tie‑breaker procedure defined in RFC 8445.
