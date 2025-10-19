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

---

## ✅ Quiz — Day 3 (10 Questions + Answers)

1) What does a codec do in VoIP?
   - Answer: Compresses/encodes audio for transmission (and decodes on receive).

2) Name one uncompressed and one modern adaptive codec from the lesson.
   - Answer: G.711 (uncompressed), Opus (adaptive).

3) Which protocol carries the media stream and which reports quality?
   - Answer: RTP carries media; RTCP reports quality (jitter, loss, RTT).

4) What is the purpose of a jitter buffer?
   - Answer: Smooth variation in packet arrival times; avoid choppiness vs added delay.

5) Roughly how much bandwidth does G.711 use per direction including overhead?
   - Answer: ~87 kbps per direction.

6) In SIP, where is codec negotiation performed?
   - Answer: In the SDP offer/answer exchange.

7) A call has one-way audio. What are two common causes?
   - Answer: NAT/firewall issues or wrong SDP address/port.

8) What Wireshark filter would you use to view both signaling and media?
   - Answer: `sip || rtp || rtcp`.

9) Why might Opus survive poor Wi‑Fi better than G.711?
   - Answer: Opus adapts bitrate/robustness and tolerates loss better.

10) If packetization increases (larger ptime), what's a tradeoff?
   - Answer: Fewer packets (lower overhead) but more latency and loss impact per packet.

## Appendix — Deep Dives

### Deep Dive: SDP Offer/Answer and Packetization (ptime)

- Why it matters: Codec choice and packetization directly affect bandwidth, latency, and loss impact. SIP endpoints negotiate these via SDP during offer/answer.
- Key details:
  - Offer/answer defines how endpoints agree on media parameters and codecs [RFC 3264 — Offer/Answer](https://www.rfc-editor.org/rfc/rfc3264).
  - SDP syntax and attributes (including `a=ptime` and `a=maxptime`) are specified in the current SDP spec [RFC 8866 — SDP](https://www.rfc-editor.org/rfc/rfc8866).
  - `ptime` indicates the preferred packetization time in milliseconds per audio packet; common values: 20 ms (G.711 default), 10–60 ms typical ranges. `maxptime` bounds the maximum.
  - Endpoints may advertise preferences but can transmit different packetization if needed; interoperability improves when both sides support 20 ms.
  - Tradeoffs: larger `ptime` lowers packets/second and overhead but adds mouth-to-ear delay and increases loss impact per packet.
- Practical example:

```sdp
v=0
o=- 0 0 IN IP4 203.0.113.10
s=Call
c=IN IP4 203.0.113.10
t=0 0
m=audio 40000 RTP/AVP 0 8 101
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=maxptime:40
```

- References: [RFC 3264 — Offer/Answer](https://www.rfc-editor.org/rfc/rfc3264), [RFC 8866 — SDP](https://www.rfc-editor.org/rfc/rfc8866)

### Deep Dive: RTCP Reports and Jitter Interpretation

- Why it matters: RTCP sender/receiver reports quantify quality (loss, jitter, RTT) and help pinpoint impairments.
- Key details:
  - RTP defines interarrival jitter and RTCP report blocks with fraction lost, cumulative lost, highest seq, jitter, and LSR/DLSR for RTT [RFC 3550 — RTP](https://www.rfc-editor.org/rfc/rfc3550).
  - Jitter is a smoothed estimator (per-packet transit variance); values are in timestamp units. Tools convert to milliseconds using the payload clock rate.
  - RTT is derived from sender/receiver report timestamps: `RTT ≈ now - LSR - DLSR` per [RFC 3550 — RTP](https://www.rfc-editor.org/rfc/rfc3550).
  - Reasonable targets: low single-digit ms jitter on wired LANs; higher on Wi‑Fi. One‑way delay budgets above ~150 ms start to be noticeable [ITU‑T G.114](https://www.itu.int/rec/T-REC-G.114/en).
  - Loss patterns matter: bursty loss degrades voice more than the same average random loss.
- Practical checklist:
  - In Wireshark: Telephony → RTP → Streams → Analyze to view jitter, loss, and clock skew; correlate with RTCP [Wireshark RTP Analysis](https://www.wireshark.org/docs/wsug_html_chunked/ChTelRTP.html).
  - Compare uplink vs downlink metrics; asymmetry often indicates access-network problems.
  - Validate that SSRCs and payload types match negotiated SDP.
- References: [RFC 3550 — RTP](https://www.rfc-editor.org/rfc/rfc3550), [ITU‑T G.114](https://www.itu.int/rec/T-REC-G.114/en), [Wireshark RTP Analysis](https://www.wireshark.org/docs/wsug_html_chunked/ChTelRTP.html)

### Deep Dive: Opus over RTP (Payload, FEC, and Packetization)

- Why it matters: Opus is a modern, adaptive codec that delivers high quality across diverse networks when configured correctly.
- Key details:
  - RTP payload format for Opus, including clock rate (48000 Hz) and channel parameters, is defined in [RFC 7587 — RTP Payload for Opus](https://www.rfc-editor.org/rfc/rfc7587).
  - Typical SDP advertises `rtpmap` and optional `fmtp` parameters such as `minptime`, `maxptime`, `stereo`, `sprop-maxcapturerate`, and `useinbandfec`.
  - In-band FEC (`useinbandfec=1`) improves robustness to burst loss; consider enabling on lossy links in combination with smaller `ptime` (e.g., 20 ms).
  - DTX can reduce bandwidth during silence but may interact with VAD/comfort-noise; test end-to-end behavior.
  - Interop notes: ensure both sides agree on stereo vs mono and honor `maxptime` ≤ 60 ms as commonly implemented.
- Example SDP offer snippet:

```sdp
m=audio 49170 RTP/AVP 111 101
a=rtpmap:111 opus/48000/2
a=fmtp:111 minptime=10;maxptime=60;useinbandfec=1
a=rtpmap:101 telephone-event/8000
```

- References: [RFC 7587 — RTP Payload for Opus](https://www.rfc-editor.org/rfc/rfc7587), [Opus Codec — Official Site](https://opus-codec.org/)
