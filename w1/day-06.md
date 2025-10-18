# Day 6 — Common PBX Platforms

## Objectives
- Compare Asterisk, FreeSWITCH, 3CX, and cloud PBXs
- Learn open-source vs commercial models

## Comparison Snapshot
- Asterisk: Mature, huge ecosystem, dialplan-centric, flexible modules (PJSIP, ARI, AMI)
- FreeSWITCH: Media powerhouse, XML dialplan, strong conferencing features
- 3CX: Commercial, Windows/Linux, GUI-driven, integrated features out of the box
- Cloud PBX (UCaaS/CPaaS): Twilio, Plivo, etc. API-first, pay-as-you-go, rapid integration

## Selection Criteria
- Feature needs (queues, recording, IVR complexity)
- Scale and stability requirements
- Integration surface (REST/Webhooks/Streaming APIs)
- Ops model (self-managed vs managed cloud)

## Hands-on (10–15 min)
1) Skim the feature pages of Asterisk and FreeSWITCH
2) List 3 reasons to choose each for a small team vs a call center

## Quick Check (Self-test)
- Which platform is most API-driven out-of-the-box?
- When would you prefer a self-hosted PBX over UCaaS?

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Asterisk — Official Site](https://www.asterisk.org) | Overview, docs, and community for the open-source IP-PBX. |
| [FreeSWITCH — Official Site](https://freeswitch.com) | Platform, docs, and commercial support options. |
| [3CX — Official Site](https://www.3cx.com) | Features list, editions, and deployment models. |
| [Twilio Elastic SIP Trunking](https://www.twilio.com/voice/sip-trunking) | Example UCaaS/CPaaS trunking service. |
| [Plivo SIP Trunking](https://www.plivo.com/sip-trunking/) | Alternative CPaaS trunk provider. |
| [Asterisk ARI](https://wiki.asterisk.org/wiki/display/AST/Asterisk+13+Application+Resource+Interface) | Build apps via REST/WS; shows API-driven potential. |
| [FreeSWITCH Mod Modules](https://freeswitch.org/confluence/display/FREESWITCH/Mod_confluence) | Module ecosystem for features and media handling. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Asterisk vs FreeSWITCH (Overview)” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+vs+FreeSWITCH) |
| “What is 3CX?” | ~5–8 min | [YouTube](https://www.youtube.com/results?search_query=What+is+3CX) |
| “UCaaS vs On-Prem PBX” | ~7–10 min | [YouTube](https://www.youtube.com/results?search_query=UCaaS+vs+on+prem+PBX) |

## Deliverable
- File: `day6_pbx_selection_matrix.md` or `.png` (table comparing platforms for your context)
- Notes: 3 bullets on your platform choice rationale (features, integrations, ops model)
- Goal: Make an informed selection based on requirements and constraints.
