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

## Further Reading
- RTP basics — `https://www.voip-info.org/rtp/`
- Opus codec — `https://opus-codec.org/`

## Deliverable
- A screenshot of your Wireshark RTP stream analysis + 3 bullet observations.
