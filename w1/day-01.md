# Day 1 — The Big Picture: What is a PBX?

## Objectives
- Understand the role of a PBX in telephony systems
- Differentiate PSTN, PBX, and IP-PBX
- See how VoIP disrupted traditional PBXs

## Key Concepts (Explained)
- PBX (Private Branch Exchange): The private phone system inside an organization. Routes internal calls and connects to external networks.
- PSTN (Public Switched Telephone Network): The global, circuit-switched telephone network.
- IP-PBX: A PBX implemented in software that uses IP networking (e.g., Asterisk, FreeSWITCH).
- SIP: Signaling protocol used to set up, modify, and tear down calls in IP telephony.

## Mental Model
A PBX is like your company’s internal air traffic control for calls. It knows who is calling, where to route the call, how to handle external lines, and how to apply features (voicemail, IVR, queues).

```
Phones/Softphones ──▶ PBX ──▶ SIP Trunk ──▶ Carrier ──▶ PSTN ──▶ Destination
              ▲         │
              └─────────┴── Internal extensions, voicemail, IVR, queues
```

## What VoIP Changed
- Switched from proprietary hardware to software running on commodity servers
- Reduced cost and increased flexibility (APIs, integrations, remote work)
- Enabled cloud telephony: UCaaS/CPaaS providers abstract carrier complexity

## Hands-on (10–15 min)
1) Sketch a basic PBX topology that includes:
   - Two internal extensions (e.g., 1000, 1001)
   - The PBX
   - A SIP trunk to a carrier
   - An external destination on the PSTN
2) Write 3 bullets on when a PBX uses the SIP trunk vs an internal route.

## Quick Check (Self-test)
- When do you need a SIP trunk?
- What’s the difference between PBX and IP-PBX?
- Which protocol sets up calls in VoIP?

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [What is a PBX? — VoIP-Info.org](https://www.voip-info.org/pbx/) | Classic overview of PBX types and how they evolved to IP. |
| [Asterisk Overview — asterisk.org](https://www.asterisk.org/asterisk/) | Official introduction to Asterisk as a leading open-source IP-PBX. |
| [FreeSWITCH Project](https://freeswitch.com) | Another open-source PBX with scalable architecture. |
| [VoIP-Info: What is SIP?](https://www.voip-info.org/sip/) | Explains SIP message flow and role in VoIP. |
| Cisco — Understanding VoIP Basics | Cisco’s modern VoIP architecture overview. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “What is a PBX? (Traditional vs VoIP)” — NetworkChuck | 9 min | [YouTube](https://www.youtube.com/results?search_query=NetworkChuck+What+is+a+PBX) |
| “PBX, IP-PBX, SIP Explained Simply” — PowerCert Animated Videos | 8 min | [YouTube](https://www.youtube.com/results?search_query=PowerCert+PBX+IP-PBX+SIP) |
| “How VoIP Works” — TechQuickie | 6 min | [YouTube](https://www.youtube.com/results?search_query=TechQuickie+How+VoIP+Works) |

## 🧾 Deliverable
- File: `day1_pbx_topology.png` or `.pdf`
- Notes: 3 bullets explaining SIP trunk usage
- Goal: Visualize your understanding of how a PBX routes internal vs external calls.
