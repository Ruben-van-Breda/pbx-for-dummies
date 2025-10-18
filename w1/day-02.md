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
