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

## Deliverable
- A diagram of a WebRTC client connecting through WSS/ICE to your PBX.
