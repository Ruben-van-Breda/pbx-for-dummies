# Day 4 — SIP Signaling Deep Dive

## Objectives
- Learn SIP message types (INVITE, ACK, BYE, REGISTER)
- Read SIP call flows and key headers

## Key Concepts (Explained)
- SIP is a text-based protocol (like HTTP) used to set up sessions.
- Core messages: INVITE (start call), 100 Trying, 180 Ringing, 200 OK, ACK (confirm), BYE (end), REGISTER (location).
- Important headers: Via (routing), From/To (caller/callee), Contact (reachable URI), Call-ID (dialog), CSeq (sequence), SDP (media offer/answer in body).

## Minimal Call Flow
```
Caller → INVITE → Callee
Caller ← 100 Trying ← Callee
Caller ← 180 Ringing ← Callee
Caller ← 200 OK (with SDP) ← Callee
Caller → ACK → Callee
[Media over RTP]
Caller → BYE → Callee
Caller ← 200 OK ← Callee
```

## Reading SIP in Wireshark
- Filter: `sip`
- Follow a call: Right-click → Follow → TCP/UDP Stream
- Expand SDP to see codec offers (m=audio, a=rtpmap)

## Hands-on (15–20 min)
1) Capture a short softphone call in Wireshark
2) Identify the first INVITE and list:
   - Request-URI
   - From/To/Contact
   - Supported codecs in SDP
3) Find the ACK; confirm Call-ID and CSeq progression

## Quick Check (Self-test)
- What’s the purpose of ACK?
- Which header tells intermediaries where to send replies?
- Where do you see the media ports and codecs?

## Further Reading
- RFC 3261 (skim Sections 1–10) — `https://www.rfc-editor.org/rfc/rfc3261`
- SIP call flows — `https://www.voip-info.org/sip-call-flows/`

## Deliverable
- A short note with the INVITE’s Request-URI, Contact, and first offered codec.
