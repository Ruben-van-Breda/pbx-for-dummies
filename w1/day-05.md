# Day 5 — Inside a PBX: Call Routing & Logic

## Objectives
- Understand dialplans and call context
- Learn how a PBX bridges two SIP dialogs

## Key Concepts (Explained)
- Registrar: Service that accepts REGISTER from endpoints and maintains bindings (user → Contact URI).
- B2BUA (Back-to-Back User Agent): PBX acts as both user agents, creating two separate call legs and bridging them.
- Dialplan: Rule set determining how calls are routed based on called number (extension), caller, time, etc.

## Example (Asterisk flavor)
```
[internal]
exten => 1000,1,Dial(PJSIP/1000)
exten => 1001,1,Dial(PJSIP/1001)

[outbound]
exten => _9X.,1,Set(NUM=${EXTEN:1})
exten => _9X.,n,Dial(PJSIP/trunk/${NUM})
```

## Bridge Concept
- Leg A: Caller ↔ PBX
- Leg B: PBX ↔ Callee (internal or external)
- PBX controls features (recording, IVR, transfer) between legs

## Hands-on (15–20 min)
1) Open a sample `extensions.conf` and identify contexts
2) Write a simple rule: internal calls 1xxx route to users; 9+number goes to trunk
3) Note: Add safeguards (e.g., block international unless whitelisted)

## Quick Check (Self-test)
- What does a registrar store?
- Why is PBX called a B2BUA?
- What’s the meaning of `_9X.` in Asterisk pattern matching?

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Asterisk Dialplan](https://wiki.asterisk.org/wiki/display/AST/Dialplan) | Official docs for dialplan concepts, priorities, and contexts. |
| [Asterisk Pattern Matching](https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching) | How `_X`, `.` and character classes work in extensions. |
| [Application: Dial()](https://wiki.asterisk.org/wiki/display/AST/Application_Dial) | Behavior, options, DIALSTATUS handling for bridging calls. |
| [Back-to-Back User Agent — Wikipedia](https://en.wikipedia.org/wiki/Back-to-back_user_agent) | What a B2BUA is and how it differs from a SIP proxy. |
| [SIP (RFC 3261)](https://www.rfc-editor.org/rfc/rfc3261) | Registrar role and SIP dialog basics (reference). |
| [PJSIP Configuration — Asterisk](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration) | Endpoint, AOR, auth, and transport objects used by the dialplan. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Asterisk Dialplan Basics” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+dialplan+basics) |
| “PJSIP Setup in Asterisk (Endpoints, AOR, Auth)” | ~7–12 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+PJSIP+configuration+endpoints+aor+auth) |
| “B2BUA vs Proxy (SIP Architecture)” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=B2BUA+vs+SIP+proxy) |

## Deliverable
- Files: `day5_dialplan_snippet.conf` (your internal/outbound example) and optional `day5_call_flow.png` (bridge diagram)
- Notes: 3 bullets on how your dialplan routes internal vs outbound calls and how failures are handled (e.g., `DIALSTATUS`, backup trunk)
- Goal: Demonstrate a minimal, safe routing logic with clear patterns and failovers.

---

## ✅ Quiz — Day 5 (10 Questions + Answers)

1) What does a registrar store after a REGISTER request?
   - Answer: The binding of a user to one or more Contact URIs (locations).

2) Why is a PBX often called a B2BUA?
   - Answer: It creates and manages two separate call legs and bridges them.

3) What is a dialplan?
   - Answer: A ruleset defining how calls are routed based on patterns/context.

4) In the Asterisk example, what does `_9X.` mean?
   - Answer: Pattern: numbers starting with 9, followed by one digit and any digits.

5) What does `${EXTEN:1}` do in the example?
   - Answer: Strips the leading 9 from the dialed extension.

6) Name one reason to add safeguards in outbound routing.
   - Answer: Prevent accidental international calls; limit by time/caller; whitelist.

7) Where do endpoint, AOR, and auth objects live for PJSIP in Asterisk?
   - Answer: In `pjsip.conf` (referenced by the dialplan).

8) Which application bridges calls in the example dialplan?
   - Answer: `Dial()`.

9) How can failures be detected in Asterisk call bridging?
   - Answer: Using `DIALSTATUS` and handling failover logic.

10) Give two items typically found in an internal context.
   - Answer: Extension-to-endpoint mappings (e.g., 1000/1001), local feature codes.

## Appendix — Deep Dives

### Deep Dive: Asterisk Dialplan Pattern Matching

- Why it matters: Precise patterns prevent misroutes and enable safe outbound rules.
- Key details:
  - `_X` matches a digit; `.` matches one or more of any digits; character classes `[ ]` constrain sets.
  - Priority sequence `1, n, n+1...`; use `same => n` for readability.
  - Use separate contexts for internal vs outbound; include contexts to compose routing.
- Practical checklist:
  - Validate with `dialplan show` and `console dial` tests; add explicit denies or whitelists for expensive routes.
- References: [Asterisk Dialplan](https://wiki.asterisk.org/wiki/display/AST/Dialplan), [Pattern Matching](https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching)

### Deep Dive: PJSIP Object Model (endpoint, aor, auth, transport)

- Why it matters: Understanding object roles simplifies provisioning and troubleshooting.
- Key details:
  - `endpoint`: media/signaling capabilities; `aor`: contact bindings; `auth`: credentials; `transport`: IP/port/TLS.
  - NAT helpers: `rewrite_contact`, `force_rport`, `rtp_symmetric` for symmetric signaling/media.
  - Link dialplan to objects via `Dial(PJSIP/endpoint)`.
- Practical checklist:
  - Verify endpoint name matches dialplan; check `pjsip show endpoint <id>`; confirm NAT flags on remote clients.
- References: [Asterisk PJSIP Configuration](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration)

### Deep Dive: Bridging and DIALSTATUS Handling

- Why it matters: Clean failure handling improves reliability and user experience.
- Key details:
  - `Dial()` returns statuses like `BUSY`, `CONGESTION`, `NOANSWER`, `CHANUNAVAIL`.
  - Implement fallback routes (backup trunk, voicemail) based on `DIALSTATUS`.
  - Mid‑call features (transfer/record) depend on PBX B2BUA control.
- Practical checklist:
  - Add post‑Dial logic; log failures; simulate carrier failover.
- References: [Application: Dial()](https://wiki.asterisk.org/wiki/display/AST/Application_Dial)
