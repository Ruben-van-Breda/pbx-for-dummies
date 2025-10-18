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

## Further Reading
- ICE overview — `https://webrtc.org/getting-started/ice`
- NAT traversal basics — `https://www.voip-info.org/nat/`

## Deliverable
- A one-page note with your candidate diagram and when you’d deploy TURN.
