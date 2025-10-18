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

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [SIP.js](https://sipjs.com) | JavaScript library for SIP over WebSocket (WSS) in browsers. |
| [Asterisk & WebRTC](https://wiki.asterisk.org/wiki/display/AST/WebRTC) | Setup notes for WebRTC with Asterisk (ICE, DTLS-SRTP, WSS). |
| [Twilio Voice (Programmable Voice)](https://www.twilio.com/voice) | CPaaS example offering WebRTC and PSTN bridging. |
| [Zoom Phone / UCaaS Overview](https://explore.zoom.us/en/zoom-phone/) | Modern UCaaS feature set reference. |
| [Plivo Voice API](https://www.plivo.com/voice/) | CPaaS alternative with SIP/WebRTC interop. |

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
