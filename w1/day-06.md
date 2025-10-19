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
| [FreeSWITCH Modules](https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Modules/) | Module ecosystem for features and media handling. |

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

---

## ✅ Quiz — Day 6 (10 Questions + Answers)

1) Name two strengths of Asterisk.
   - Answer: Large ecosystem; flexible dialplan; modules like PJSIP/ARI/AMI.

2) Name two strengths of FreeSWITCH.
   - Answer: Strong media handling/conferencing; XML dialplan; scalable design.

3) What is a key characteristic of 3CX deployments?
   - Answer: Commercial, GUI-driven, integrated features out of the box.

4) What defines UCaaS/CPaaS providers?
   - Answer: API-first cloud platforms with pay-as-you-go telephony features.

5) When might you prefer self-hosted PBX over UCaaS?
   - Answer: Specific control/compliance needs, cost at scale, custom integrations.

6) List two selection criteria when choosing a PBX platform.
   - Answer: Feature needs; integration surface; stability/scale; ops model.

7) Which option is most API-driven out-of-the-box?
   - Answer: UCaaS/CPaaS platforms.

8) What does ARI enable for Asterisk?
   - Answer: Application control via REST/WebSockets for custom call logic.

9) Give one reason to choose FreeSWITCH for a call center.
   - Answer: High-performance media features and conferencing capabilities.

10) What artifact should you produce for this day’s deliverable?
   - Answer: A selection matrix comparing platforms for your context.

## Appendix — Deep Dives

### Deep Dive: Architecture — Asterisk vs FreeSWITCH

- Why it matters: Internal architecture influences features, scaling, and integration patterns.
- Key details:
  - Asterisk: application/dialplan centric, channel drivers (PJSIP), ARI/AMI control surfaces.
  - FreeSWITCH: media switch core, XML dialplan, strong conferencing/mixing.
  - Both act as B2BUAs; choose based on feature fit and ecosystem.
- References: [Asterisk — Official](https://www.asterisk.org), [FreeSWITCH — Modules](https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Modules/)

### Deep Dive: Licensing and Ecosystem

- Why it matters: License and ecosystem affect deployment models and extensions.
- Key details:
  - Asterisk: GPLv2 with commercial add‑ons available via partners.
  - FreeSWITCH: MPL 2.0; commercial support via vendors.
  - 3CX: commercial licensing; integrated apps; closed source.
- References: [Asterisk License](https://www.asterisk.org/license/), [FreeSWITCH License](https://freeswitch.com/legal/), [3CX Licensing](https://www.3cx.com/ordering/pricing/)

### Deep Dive: UCaaS/CPaaS Tradeoffs vs On‑Prem

- Why it matters: Align platform choice with ops, compliance, and integration needs.
- Key details:
  - Cloud: rapid provisioning, APIs, global scale; vendor constraints, egress costs, data residency.
  - On‑prem: control/compliance, custom media paths; operational burden.
  - Hybrid: SBCs and SIP trunks bridge models.
- References: [Twilio Elastic SIP Trunking](https://www.twilio.com/voice/sip-trunking), [Plivo SIP Trunking](https://www.plivo.com/sip-trunking/)
