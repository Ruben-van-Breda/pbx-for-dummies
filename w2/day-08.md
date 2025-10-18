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
