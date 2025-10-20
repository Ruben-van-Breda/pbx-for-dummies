# Day 2 — Telephony 101: From Analog to Digital

## Objectives
- Understand analog vs digital lines and signaling
- Learn FXS/FXO, T1/E1, and trunking
- Distinguish call setup from media transport

## Key Concepts (Explained)
- FXS (Foreign eXchange Station): Port that delivers analog line to a phone (provides battery and dial tone).
- FXO (Foreign eXchange Office): Port that connects a device to the PSTN or FXS port (receives battery and dial tone).
- T1/E1 (Digital trunks): Bundles of digital channels (T1=24 ch in NA; E1=32 ch in EU), used before SIP trunks became common.
- Trunk: A high-capacity link carrying multiple calls between systems.
- Call setup vs transport: Setup is signaling (e.g., SIP/ISDN); transport is voice frames (e.g., RTP/TDM).

<video controls src="../Telephony__Analog_to_Digital.mp4" title="Title"></video>

## Details: FXS/FXO, PRI (T1/E1), and Analog↔IP Gateways

### FXS vs FXO (Analog Interfaces)
- What it is and why it matters: FXS provides battery and dial tone to analog phones; FXO receives that service to connect devices toward the PSTN. Choosing the correct side avoids wiring errors and no-dial-tone issues.
- Key details:
  - FXS → Phone/ATA; FXO → PSTN/FXS.
  - Ring voltage, polarity, and impedance vary by region; verify device specs.
  - Caller ID delivery methods (FSK/DTMF) and timing differ by country; align with gateways.
- Checklist: Identify the power-supplying side; match regional signaling; test ring cadence and caller ID.
- References: [FXS/FXO — VoIP-Info.org](https://www.voip-info.org/fxs-fxo/)

### PRI over T1/E1
- What it is and why it matters: PRI provided multi-channel digital trunks before SIP became ubiquitous. Understanding channelization and signaling helps with legacy migrations and troubleshooting.
- Key details:
  - T1 PRI: 23B+1D (24 timeslots); E1 PRI: 30B+1D+1 framing slot (32 timeslots).
  - Signaling: ISDN Q.931 on the D-channel; framing/line defined by G.704/G.703 (E1).
  - Common gotchas: CRC/framing mismatches, clock slips, and profile differences (NI2 vs EuroISDN).
- Checklist: Confirm framing/CRC/clocking, switch type, and channel map; test inbound/outbound call setup.
- References: [ITU-T Q.931 (ISDN DSS1)](https://www.itu.int/rec/T-REC-Q.931), [ITU-T G.704 (E1 framing)](https://www.itu.int/rec/T-REC-G.704), [ITU-T G.703](https://www.itu.int/rec/T-REC-G.703), [Asterisk DAHDI/PRI Overview](https://wiki.asterisk.org/wiki/display/DAHDI/DAHDI+Telephony+Interfaces)

### Analog↔IP Gateways (ATAs and Media Gateways)
- What it is and why it matters: Gateways and ATAs bridge legacy analog phones/faxes, door phones, and alarms into IP PBXs. They enable phased migrations and preserve critical analog endpoints.
- Key details:
  - ATA ports are FXS (to analog device); upstream SIP to PBX/ITSP.
  - Fax: prefer T.38 when supported; otherwise G.711 pass-through with careful jitter/ptime.
  - Dial-plan nuances: hook-flash, pulse-to-tone conversion, and caller ID timing.
- Checklist: Map ports to extensions/DIDs; set codec policy (G.711 for fax); test hook-flash features and failover.
- References: [Analog Telephone Adapter (ATA) — VoIP-Info.org](https://www.voip-info.org/ata/), [Asterisk PJSIP Examples](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration+Examples)

## Visual
```
Analog phone ──(FXS)── PBX FXO card ── PSTN

PBX ──(T1/E1 PRI)── Telco switch  (multiple voice channels)
```

## Modern Context
- Many deployments replaced T1/E1 with SIP trunks over IP
- Gateways still exist to bridge legacy sites or analog endpoints to IP

## Hands-on (10–15 min)
1) Watch a quick explainer on analog vs VoIP.
2) Draw two mini-diagrams:
   - FXS→Phone and FXO→PSTN
   - PBX→PRI→Telco
3) Note when you’d still need analog (fax lines, door phones, legacy alarms).

## Quick Check (Self-test)
- Which side provides battery and dial tone: FXS or FXO?
- How many channels in T1 vs E1?
- Why do some sites still keep a few analog lines?

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [FXS/FXO — VoIP-Info.org](https://www.voip-info.org/fxs-fxo/) | Clear primer on analog interfaces and typical use cases. |
| [Primary Rate Interface (PRI) — VoIP-Info.org](https://www.voip-info.org/pri/) | Overview of PRI signaling and channelization. |
| [Plain Old Telephone Service (POTS) — Wikipedia](https://en.wikipedia.org/wiki/Plain_old_telephone_service) | Background on analog telephony, tip/ring, battery. |
| [Primary Rate Interface — Wikipedia](https://en.wikipedia.org/wiki/Primary_Rate_Interface) | Standards and regional differences (T1 vs E1). |
| [T-carrier — Wikipedia](https://en.wikipedia.org/wiki/T-carrier) | History and specs for T1 lines. |
| [E-carrier — Wikipedia](https://en.wikipedia.org/wiki/E-carrier) | History and specs for E1 lines. |
| [Analog Telephone Adapter (ATA) — VoIP-Info.org](https://www.voip-info.org/ata/) | How ATAs bridge analog devices to IP networks. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “FXS vs FXO Explained” — PowerCert Animated Videos | ~8 min | [YouTube](https://www.youtube.com/results?search_query=PowerCert+FXS+FXO) |
| “Analog vs VoIP Explained” | ~9 min | [YouTube](https://www.youtube.com/results?search_query=Analog+vs+VoIP+explained) |
| “PRI vs SIP Trunking” | ~7–10 min | [YouTube](https://www.youtube.com/results?search_query=PRI+vs+SIP+trunking) |

## 🧾 Deliverable
- File: `day2_analog_digital_diagrams.png` or `.pdf`
- Notes: 3 bullets on when to keep analog, who provides dial tone (FXS vs FXO), and when PRI is preferable to SIP trunking (e.g., legacy constraints, regulatory, last-mile reliability).
- Goal: Visualize analog and digital call paths and when each is used.

---

## ✅ Quiz — Day 2 (20 Questions + Answers)

1) What does an FXS port provide to the attached device?
   - Answer: Battery power and dial tone (delivers analog line to the phone).

2) Which interface would you use to connect a PBX analog card to the PSTN?
   - Answer: FXO (receives battery and dial tone from the telco or FXS port).

3) How many channels are in a T1 vs an E1?
   - Answer: T1 has 24 channels (NA); E1 has 32 channels (EU).

4) What is a trunk in telephony terms?
   - Answer: A high-capacity link carrying multiple calls between systems.

5) Which handles call setup and which carries media: SIP/ISDN vs RTP/TDM?
   - Answer: SIP/ISDN handle setup (signaling); RTP/TDM carry the media.

6) Give one reason sites still keep a few analog lines.
   - Answer: Legacy devices (fax, door phones, alarms) or regulatory/emergency fallback.

7) What modern technology commonly replaced T1/E1 trunks?
   - Answer: SIP trunks over IP.

8) In the visual, which side of the analog connection faces the phone?
   - Answer: FXS → Phone.

9) When bridging legacy analog to IP, what device is typically used?
   - Answer: An Analog Telephone Adapter (ATA) or a media gateway.

10) If you need 60 concurrent calls on-prem without SIP, how many T1s are required?
   - Answer: Three T1s (3 × 24 = 72 channels, allowing for overhead/expansion).

11) Which side supplies battery and dial tone in an analog pair?
   - Answer: FXS supplies; FXO receives.

12) What are the typical PRI channel counts for T1 and E1?
   - Answer: T1: 23B+1D (24 slots). E1: 30B+1D+1 (32 slots).

13) Which protocol carries signaling on the PRI D-channel?
   - Answer: ISDN Q.931 (DSS1).

14) Name two common causes of PRI link flapping.
   - Answer: Framing/CRC mismatches and clocking slips.

15) On an ATA, what port type connects to an analog phone?
   - Answer: FXS port on the ATA.

16) Preferred method for fax over IP when supported?
   - Answer: T.38.

17) If T.38 is unavailable, which codec is commonly used for fax pass-through?
   - Answer: G.711 (µ-law or A-law) with stable jitter and packetization.

18) What does hook-flash enable on some analog devices after IP migration?
   - Answer: Features like call transfer or three-way calling via gateway-detected flash.

19) What should you verify when mapping DIDs to analog ports on a gateway?
   - Answer: Correct port-to-extension/DID mapping and inbound routing rules.

20) Which ITU-T recommendations define E1 framing and physical characteristics?
    - Answer: G.704 (framing) and G.703 (physical/electrical).

## Appendix — Deep Dives

### Deep Dive: PRI Signaling and Framing (Q.931, G.704)

- Why it matters: Correct framing and signaling are essential for stable T1/E1 PRI links during migrations.
- Key details:
  - D‑channel carries Q.931 call control (SETUP/CONNECT/RELEASE) for channel allocation.
  - T1: 23B+1D; E1: 30B+1D(+1 framing). Framing/CRC/clocking must match the telco [ITU‑T G.704/G.703].
  - Common issues: slips, CRC errors, NI2 vs EuroISDN profile mismatches.
- Practical checklist:
  - Verify line coding, framing, CRC, and switch type; monitor alarms/slips; confirm channel maps.
- References: [ITU‑T Q.931](https://www.itu.int/rec/T-REC-Q.931), [ITU‑T G.704](https://www.itu.int/rec/T-REC-G.704), [ITU‑T G.703](https://www.itu.int/rec/T-REC-G.703)

### Deep Dive: Fax over IP — T.38 vs G.711 Pass‑Through

- Why it matters: Fax reliability depends on choosing the right transport over lossy IP links.
- Key details:
  - T.38 relays fax data, avoiding PCM impairments; requires endpoint/gateway support [ITU‑T T.38].
  - G.711 pass‑through can work on clean networks; sensitive to jitter/loss/ptime choices.
  - Interop: negotiate per call; some gateways can switch to T.38 mid‑call on CED/ANS detection.
- Practical checklist:
  - Prefer T.38 when available; otherwise use G.711 µ/A‑law, stable jitter buffers, 20 ms `ptime`.
- References: [ITU‑T T.38](https://www.itu.int/rec/T-REC-T.38/en), [Asterisk T.38 Gateway](https://wiki.asterisk.org/wiki/display/AST/T.38+Gateway)

### Deep Dive: Analog Caller ID Signaling (FSK vs DTMF)

- Why it matters: CID delivery differences impact ATAs/gateway configs across regions.
- Key details:
  - Many regions use Bellcore/ETSI FSK between first/second ring; others use DTMF post‑ring.
  - Timing/leveling and polarity can affect decode success; configure per locale.
  - Gateway features may include polarity reversal and ring cadence control.
- Practical checklist:
  - Set country profile on ATAs; test CID with local carriers and adjust timing.
- References: [Caller ID — Wikipedia](https://en.wikipedia.org/wiki/Caller_ID), [ETSI EN 300 659 (Caller ID over FSK)](https://www.etsi.org/deliver/etsi_en/300600_300699/30065901/01.03.01_60/en_30065901v010301p.pdf)

 
