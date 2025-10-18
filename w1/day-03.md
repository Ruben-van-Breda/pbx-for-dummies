# Day 3 — VoIP Basics: Codecs, Packets & Media

## Objectives
- Learn how voice is digitized and transmitted
- Understand RTP/RTCP and codec negotiation
- Recognize jitter, latency, and packet loss impacts

## Key Concepts (Explained)
- Codec: Algorithm that compresses audio. Common: G.711 (64 kbps, uncompressed), Opus (adaptive, modern), G.729 (compressed, licensed in some regions).
- RTP: Carries audio packets (media stream) with sequence numbers and timestamps.
- RTCP: Companion control protocol that reports quality (jitter, loss, round-trip time).
- Jitter buffer: Smooths packet timing variation; too small leads to choppiness, too big adds delay.

## Call Flow Snapshot
1) Endpoints negotiate codecs in SIP (offer/answer, SDP)
2) Media flows directly between endpoints (or via PBX/SBC) over RTP/UDP
3) RTCP reports help diagnose quality issues

## Practical Bitrates (rule of thumb)
- G.711: ~87 kbps per direction including IP/UDP/RTP overhead
- G.729: ~31–40 kbps per direction depending on packetization
- Opus: Variable (6–40+ kbps), resilient to loss

## Hands-on: Capture RTP (15–20 min)
1) Start Wireshark. Filter: `sip || rtp || rtcp`
2) Make/receive a VoIP call (softphone to softphone or lab capture file)
3) Use Telephony → RTP → Streams → Analyze to inspect jitter and loss
4) Export audio and play it back to verify media path

## Troubleshooting Hints
- One-way audio: Usually NAT/firewall or wrong SDP address/port
- No audio: Codec mismatch or blocked UDP
- Choppy audio: Jitter buffer too small or network congestion

## Quick Check (Self-test)
- What carries media: SIP or RTP?
- Which protocol reports quality metrics?
- Why might Opus survive poor Wi-Fi better than G.711?

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [RTP (RFC 3550) — IETF](https://www.rfc-editor.org/rfc/rfc3550) | Core RTP spec (media transport, sequence, timestamps, RTCP). |
| [RTP Profile (RFC 3551) — IETF](https://www.rfc-editor.org/rfc/rfc3551) | Payload type mappings and profile for audio/video conferences. |
| [SDP (RFC 4566) — IETF](https://www.rfc-editor.org/rfc/rfc4566) | Session Description Protocol used in SIP offers/answers. |
| [RTP basics — VoIP-Info.org](https://www.voip-info.org/rtp/) | Practical overview of RTP/RTCP in VoIP. |
| [Codecs overview — VoIP-Info.org](https://www.voip-info.org/codecs/) | Common VoIP codecs, pros/cons and bandwidth usage. |
| [Opus codec — opus-codec.org](https://opus-codec.org/) | Official Opus documentation and resources. |
| [Wireshark: Analyze RTP](https://www.wireshark.org/docs/wsug_html_chunked/ChTelRTP.html) | Guide to inspecting RTP streams and QoS metrics. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “RTP and RTCP Explained” — PowerCert Animated Videos | ~8–10 min | [YouTube](https://www.youtube.com/results?search_query=PowerCert+RTP+RTCP+explained) |
| “VoIP Codecs Explained (G.711 vs Opus vs G.729)” | ~7–10 min | [YouTube](https://www.youtube.com/results?search_query=VoIP+codecs+explained+G.711+Opus+G.729) |
| “Wireshark RTP Analysis Tutorial” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Wireshark+RTP+analysis+tutorial) |

## 🧾 Deliverable
- File: `day3_rtp_analysis.png` (RTP Streams analysis screenshot); optional: `day3_call_capture.pcapng`
- Notes: 3–5 bullets including: chosen codec and packetization (ptime), average jitter/loss from analysis, any RTCP-reported RTT, and whether media was direct or anchored by the PBX/SBC.
- Goal: Validate codec negotiation and assess real call quality with measurable metrics.
