# Day 14 — PBX in 2025: Cloud, UCaaS & WebRTC

## Objectives
- Understand modern PBX evolution
- Compare on-prem vs cloud approaches

## Key Concepts (Explained)
- UCaaS: Unified Communications as a Service (hosted PBX + collaboration)
- CPaaS: Communications Platform as a Service (APIs for calls/messaging)
- SBC-aaS: Managed SBC services for secure SIP at the edge
- WebRTC: Browser-native media; integrates with SIP via gateways or SIP over WebSocket

## Integration Patterns
- SIP trunk to UCaaS/CPaaS
- SIP over WebSocket (SIP.js) to PBX/SBC with WSS and ICE
- Media gateways between WebRTC and SIP/RTP

## Hands-on (optional)
- Research how SIP.js registers over WSS and how ICE candidates flow
- Sketch an architecture that connects a browser client to your PBX

## Quick Check (Self-test)
- When would you choose UCaaS vs self-host PBX?
- What role does a WebRTC gateway or SBC play?

## Common Pitfalls
- HTTPS/WSS and certs: Browsers require valid certs for WSS/mic access; self-signed causes failures.
- Permissions/autoplay: Microphone/camera not granted, or autoplay blocked; adjust site permissions.
- ICE servers unreachable: Corporate firewalls block STUN/TURN; provide reachable TURN.
- Codec/gateway mismatch: WebRTC prefers Opus; SIP endpoint may be G.711 → plan for transcoding.
- CORS/WebSocket upgrades: WSS blocked or CORS misconfigured; ensure correct origin and proxy rules.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [SIP.js](https://sipjs.com) | JavaScript library for SIP over WebSocket (WSS) in browsers. |
| [Asterisk & WebRTC](https://wiki.asterisk.org/wiki/display/AST/WebRTC) | Setup notes for WebRTC with Asterisk (ICE, DTLS-SRTP, WSS). |
| [Twilio Voice (Programmable Voice)](https://www.twilio.com/voice) | CPaaS example offering WebRTC and PSTN bridging. |
| [Zoom Phone / UCaaS Overview](https://explore.zoom.us/en/zoom-phone/) | Modern UCaaS feature set reference. |
| [Plivo Voice API](https://www.plivo.com/voice/) | CPaaS alternative with SIP/WebRTC interop. |

## Industry Example: Siperb — WebRTC Softphone + Proxy

Siperb is a modern softphone powered by WebRTC that can work with Asterisk, FreeSWITCH, or any SIP/VoIP PBX. It provides both a browser/mobile/desktop client and a hosted WebRTC-to-SIP proxy that bridges browsers to legacy SIP systems.

- Modes of connectivity (choose per environment):
  - Socket Registration Mode: Client registers directly to your PBX over WSS/SRTP. Push notifications and some proxy features are limited.  
  - Proxy Registration Mode (Passthrough): Device registers to Siperb; you create a “connection” to your PBX/ITSP. If your side supports ICE + SRTP/DTLS, media can flow directly, minimizing media hops.  
  - Proxy Registration Mode (Transcoding): For legacy PBXs without SRTP/DTLS. Media is transcoded from WebRTC (Opus over DTLS-SRTP) to G.711 RTP.

- Notable features to evaluate:
  - Auto‑provisioned WebRTC devices (reduced password exposure)
  - Mobile & web push notifications
  - DTLS↔RTP transcoding options
  - Call recording, transfers, and 3‑way client‑side conferencing

- Where it fits in your architecture:
  - Use as the browser softphone for labs and PoCs
  - Use the proxy when your PBX isn’t fully WebRTC‑ready, or when you want simplified onboarding with provisioning and push
  - Use direct socket registration if you already operate a secure WSS + ICE + TURN stack and want end‑to‑end media without a proxy

References: [Siperb](https://www.siperb.com/) · [Siperb Knowledge Base](https://www.siperb.com/kb/)

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “SIP over WebSocket (SIP.js) Intro” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=SIP+over+WebSocket+SIP.js) |
| “WebRTC + Asterisk Overview” | ~7–12 min | [YouTube](https://www.youtube.com/results?search_query=WebRTC+Asterisk+overview) |
| “UCaaS vs CPaaS” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=UCaaS+vs+CPaaS) |

## Deliverable
- File: `day14_webrtc_pbx_architecture.png` (architecture diagram)
- Notes: How browser connects (WSS), how media is secured (DTLS-SRTP), and where SBC/gateway sits
- Goal: A clear architecture for connecting WebRTC clients to PBX/CPaaS safely.

---

## ✅ Quiz — Day 14 (10 Questions + Answers)

1) What is UCaaS?
   - Answer: Unified Communications as a Service — hosted PBX + collaboration tools.

2) What is CPaaS?
   - Answer: Communications Platform as a Service — APIs for voice/messaging/video.

3) How does WebRTC integrate with SIP systems?
   - Answer: Via gateways or SIP over WebSocket (WSS) and ICE/TURN for media.

4) Why are valid certs required for browser media?
   - Answer: Browsers require HTTPS/WSS and valid certs for mic/camera permissions.

5) What role does an SBC or SBC-aaS play in modern architectures?
   - Answer: Security, NAT traversal, topology hiding, and policy enforcement at the edge.

6) Why plan for transcoding with WebRTC?
   - Answer: WebRTC prefers Opus while SIP endpoints often use G.711.

7) What are typical pitfalls when enabling SIP over WebSocket?
   - Answer: CORS misconfig, WSS upgrade failures, and certificate issues.

8) Why might ICE servers be unreachable in corporate environments?
   - Answer: Firewalls block STUN/TURN; need reachable TURN servers.

9) When would you choose UCaaS over self-hosted PBX?
   - Answer: Faster rollout, managed operations, and integrated collaboration features.

10) What should your architecture diagram highlight for WebRTC↔PBX?
   - Answer: WSS signaling path, ICE/TURN servers, DTLS-SRTP media, and SBC/gateway placement.
