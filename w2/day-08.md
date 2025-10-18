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
