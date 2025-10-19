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

---

## ✅ Quiz — Day 1 (10 Questions + Answers)

1) What does PBX stand for, and what is its primary role?
   - Answer: Private Branch Exchange; it routes internal calls and connects to external networks.

2) Which global network do traditional phone calls traverse outside an organization?
   - Answer: The Public Switched Telephone Network (PSTN).

3) What is an IP-PBX, and how does it differ from a traditional PBX?
   - Answer: A software-based PBX that runs over IP networks; unlike traditional hardware PBXs, it uses standard servers and IP signaling/media.

4) Which signaling protocol is commonly used to set up, modify, and tear down VoIP calls?
   - Answer: SIP (Session Initiation Protocol).

5) In the mental model diagram, when would a call go from the PBX to the SIP trunk rather than stay internal?
   - Answer: When the destination is outside the organization (e.g., to the PSTN/external numbers).

6) Name two ways VoIP disrupted traditional PBXs.
   - Answer: Shifted from proprietary hardware to software on commodity servers; reduced costs and enabled flexible integrations/APIs and remote work.

7) What do UCaaS/CPaaS providers abstract for customers in cloud telephony?
   - Answer: Carrier complexity (e.g., trunks, scaling, routing, compliance) via cloud services and APIs.

8) Give two typical internal PBX features mentioned in Day 1.
   - Answer: Voicemail and IVR (also queues).

9) In a basic PBX topology, list four components you should include.
   - Answer: Two internal extensions (e.g., 1000, 1001), the PBX, a SIP trunk to a carrier, and an external PSTN destination.

10) Why is SIP specifically called a signaling protocol in VoIP?
   - Answer: Because it handles call setup/teardown and session control, not the actual media (which is typically carried via RTP).

## Appendix — Deep Dives

### Deep Dive: SIP & SDP
- What it is and why it matters: SIP sets up, modifies, and tears down calls; SDP describes the media (codecs, ports, transport). Together they negotiate how endpoints will actually exchange audio.
- Key details:
  - Core SIP methods: INVITE/ACK/BYE, REGISTER; responses 1xx–6xx; URIs like `sip:1000@example.com`.
  - SDP includes `m=audio`, codec lists, and transport attributes; offers/answers are typically in INVITE/200 OK.
  - Early media often uses 183 Session Progress; reliable provisional responses may use PRACK.
  - Interop tips: match codecs, DTMF via RFC 2833/4733, set correct `From`/`Contact` domains.
- Example (minimal flow): INVITE (SDP offer) → 100 Trying → 180 Ringing → 200 OK (SDP answer) → ACK; media flows via RTP per SDP.
- References: [RFC 3261 (SIP, 2002)](https://www.rfc-editor.org/rfc/rfc3261), [RFC 8866 (SDP, 2021)](https://www.rfc-editor.org/rfc/rfc8866), [Asterisk PJSIP Configuration Examples](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration+Examples)

### Deep Dive: SIP Trunking
- What it is and why it matters: A SIP trunk is the PBX’s connection to an ITSP/carrier for external calling. It replaces legacy PRI/analog lines with IP-based sessions.
- Key details:
  - Models: registration-based vs static-IP peering; DIDs map inbound to extensions/IVRs.
  - Security: prefer TLS for signaling and SRTP for media when supported; restrict by IP; use SBCs when available.
  - Interop: caller ID via P-Asserted-Identity/From; normalize numbers to E.164; agree on codecs (G.711 µ/A-law common).
  - Reliability: carrier failover IPs, DNS SRV, SIP timers; plan 911/E911 routing and location data.
- Checklist:
  - Collect carrier SIP endpoints/IPs, credentials, DID list, codec policy, SRTP/TLS requirements, and fax/T.38 needs.
  - Configure outbound dial plan (local/long-distance/international), inbound DID routing, and failover destinations.
- References: [SIPconnect 2.0 — SIP Forum](https://www.sipforum.org/activities/sipconnect/), [Asterisk Trunking Introduction](https://wiki.asterisk.org/wiki/display/AST/Trunking+Introduction)

### Deep Dive: RTP Media Path
- What it is and why it matters: RTP carries the audio; it is separate from SIP signaling. Correct RTP handling ensures quality and interoperability across NATs and firewalls.
- Key details:
  - RTP typically uses UDP across a port range; media may flow directly endpoint↔endpoint or via the PBX.
  - Security: SRTP (RFC 3711) with keying via SDES or DTLS-SRTP; align with endpoint support.
  - Quality: jitter buffers, packet loss concealment; prefer symmetric RTP; consider disabling direct media if NAT/SBC constraints exist.
  - Firewall/NAT: open the PBX RTP range; use ICE/rtcp-mux/WebRTC when relevant; confirm codec payload types match SDP.
- Checklist (PBX-focused):
  - Verify RTP port range and firewall rules; set symmetric RTP; choose allowed codecs; enable SRTP when supported.
  - Test early media and DTMF; capture with `pcap`/SIP traces when troubleshooting.
- References: [RFC 3550 (RTP, 2003)](https://www.rfc-editor.org/rfc/rfc3550), [RFC 3711 (SRTP, 2004)](https://www.rfc-editor.org/rfc/rfc3711), [Asterisk RTP Configuration](https://wiki.asterisk.org/wiki/display/AST/RTP+Configuration)
