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

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [RFC 3261 — SIP: Session Initiation Protocol](https://www.rfc-editor.org/rfc/rfc3261) | Authoritative SIP specification (skim Sections 1–10 today). |
| [RFC 3264 — Offer/Answer with SDP](https://www.rfc-editor.org/rfc/rfc3264) | How endpoints negotiate media (SDP) during SIP calls. |
| [SIP Call Flows — VoIP-Info.org](https://www.voip-info.org/sip-call-flows/) | Common call flow examples and ladder diagrams. |
| [SIP Headers — VoIP-Info.org](https://www.voip-info.org/sip-headers/) | Concise explanations of major SIP headers. |
| [SIP Responses — VoIP-Info.org](https://www.voip-info.org/sip-responses/) | Status code classes and meanings (1xx–6xx). |
| [Wireshark: SIP Display Filter Reference](https://www.wireshark.org/docs/dfref/s/sip.html) | Filter fields and how Wireshark parses SIP. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “SIP Call Flow Explained” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=SIP+call+flow+explained) |
| “SIP Headers Explained” | ~7–10 min | [YouTube](https://www.youtube.com/results?search_query=SIP+headers+explained) |
| “SIP + SDP Offer/Answer” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=SIP+SDP+offer+answer+explained) |
| “Wireshark SIP Analysis Tutorial” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Wireshark+SIP+analysis+tutorial) |

## 🧾 Deliverable
- File: `day4_invite_headers.png` (screenshot of INVITE headers) or `day4_sip_trace.pcapng`
- Notes: Request-URI, From/To/Contact, first offered codec in SDP, Via branch parameter, and CSeq numbers for INVITE and ACK.
- Goal: Build fluency in reading SIP messages and mapping headers to call flow stages.

---

## ✅ Quiz — Day 4 (10 Questions + Answers)

1) What is SIP used for in VoIP?
   - Answer: Signaling to set up, modify, and tear down sessions (calls).

2) List three core SIP requests involved in a basic call.
   - Answer: INVITE, ACK, BYE (also REGISTER for location).

3) What header helps route replies back to the sender through intermediaries?
   - Answer: Via.

4) Where are codecs and media ports advertised in SIP messages?
   - Answer: In the SDP body (m=audio, a=rtpmap, c= connection info).

5) In a successful call setup, which response typically precedes 200 OK?
   - Answer: 180 Ringing (often after 100 Trying).

6) What is the purpose of the ACK after a 200 OK to INVITE?
   - Answer: Confirms session establishment; completes the initial dialog handshake.

7) Name two key identifiers for a SIP dialog.
   - Answer: Call-ID and CSeq (with From/To tags as dialog identifiers).

8) Which message ends a call and what response confirms it?
   - Answer: BYE ends the call; 200 OK confirms.

9) In Wireshark, which display filter shows only SIP?
   - Answer: `sip`.

10) If media is negotiated but there is no audio, give two likely causes.
   - Answer: Codec mismatch or UDP/RTP blocked by firewall/NAT.

## Appendix — Deep Dives

### Deep Dive: SIP Transactions vs Dialogs (why both matter)

- Why it matters: Understanding the transaction layer (request–response over a single branch) vs the dialog layer (longer-lived peer relationship) helps debug retransmissions, forking, and mid-dialog requests.
- Key details:
  - A transaction spans a request and all responses to it (through completion). INVITE server/client transactions have special timer behavior [RFC 3261].
  - A dialog is established by matching Call-ID, local/remote tags, and route set; it persists across multiple transactions (e.g., re-INVITE, BYE) [RFC 3261].
  - Forking can create multiple early dialogs; only one typically answers 200 OK. Others must be terminated.
  - CSeq increases per transaction within a dialog; mismatches cause 500/491/Out-of-order errors.
- Practical checklist:
  - In traces, group by Call-ID and From/To tags to follow a dialog; use Via branch to follow a transaction.
  - Confirm CSeq increments on re-INVITE/UPDATE; verify Route set is respected for mid-dialog requests.
- References: [RFC 3261 — SIP](https://www.rfc-editor.org/rfc/rfc3261)

### Deep Dive: Via branch, magic cookie, and rport (reply routing/NAT)

- Why it matters: Correct reply routing prevents stray responses and fixes many NAT traversal issues in request/response paths.
- Key details:
  - The Via branch parameter must start with magic cookie `z9hG4bK` and be unique per branch [RFC 3261].
  - The `rport` parameter requests that responses be sent back to the source port from which the request came (useful behind NAT) [RFC 3581].
  - Proxies add their own Via at the top and remove it when forwarding responses; mismatched or reused branches break transactions.
  - Combine with SIP `Contact` and outbound keepalives for stable registrations.
- Practical example (Via header excerpt):

```
Via: SIP/2.0/UDP 198.51.100.10;branch=z9hG4bK-a1b2c3;rport
```

- References: [RFC 3261 — SIP](https://www.rfc-editor.org/rfc/rfc3261), [RFC 3581 — rport](https://www.rfc-editor.org/rfc/rfc3581)

### Deep Dive: Early Media (180 vs 183) and Reliable Provisionals (PRACK)

- Why it matters: Ringback and announcements before answer depend on early media; reliability avoids SDP mismatch and clipping on lossy links.
- Key details:
  - 180 Ringing typically implies local ringback; 183 Session Progress may carry SDP for early media [RFC 3960].
  - Reliable provisional responses use `Require: 100rel` and PRACK to confirm receipt and allow offer/answer before 200 OK [RFC 3262].
  - Early dialogs exist prior to final response; ensure consistent SDP direction and ports to avoid one-way early audio.
  - Gateways/SBCs often anchor early media; verify RTP actually flows when 183+SDP is seen.
- Practical checklist:
  - When early media is expected, look for 183 with SDP and confirm PRACK if `100rel` is used.
  - Check that final 200 OK SDP matches or properly renegotiates from provisional SDP.
- References: [RFC 3960 — Early Media](https://www.rfc-editor.org/rfc/rfc3960), [RFC 3262 — PRACK](https://www.rfc-editor.org/rfc/rfc3262)
