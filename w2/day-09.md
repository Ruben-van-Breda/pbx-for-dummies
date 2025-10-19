# Day 9 — Call Routing & Dialplans

## Objectives
- Write simple inbound/outbound routing rules
- Handle extensions and basic voicemail

## Concepts (Explained)
- Inbound routes: Match DID or trunk, send to IVR/queue/extension
- Outbound routes: Match dial patterns and choose a trunk
- Failover: Try alternate trunks or messages on failure

## Example (Asterisk-style)
```
[from-trunk]
exten => _+1NXXNXXXXXX,1,Goto(inbound,${EXTEN},1)

[inbound]
exten => _+1NXXNXXXXXX,1,Answer()
 same => n,Goto(ivr-main,s,1)

[outbound]
exten => _9NXXNXXXXXX,1,Set(NUM=${EXTEN:1})
 same => n,Dial(PJSIP/trunk_primary/${NUM},30)
 same => n,ExecIf($["${DIALSTATUS}"!="ANSWER"]?Dial(PJSIP/trunk_backup/${NUM},30))
 same => n,Hangup()
```

## Hands-on (15–20 min)
1) Define an outbound pattern for local calls and block international by default
2) Add an inbound context that sends to IVR (prepared in Day 12)
3) Add simple voicemail for extension 1000 (to be enabled on Day 12)

## Quick Check (Self-test)
- What does `_9NXXNXXXXXX` match?
- Where would you implement least-cost routing?

## Common Pitfalls
- Overlapping/ambiguous patterns: Unexpected matches; prefer specific patterns before broad ones.
- Route loops: Goto between contexts without termination conditions; ensure clear end states.
- Missing normalization: Carriers reject non‑E.164 numbers; normalize on outbound and map inbound DIDs.
- Emergency/Special numbers: Pattern blocks critical calls; add explicit exceptions and least‑restrictive routes.
- No failover/timeouts: Dial without alternate trunk or timeout → long silence for callers; add failbacks.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Pattern Matching — Asterisk](https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching) | Pattern syntax (`_`, ranges, `.`) for dialplan routes. |
| [Dialplan Hints and Extensions](https://wiki.asterisk.org/wiki/display/AST/Extensions+and+Contexts) | How contexts and extensions are structured. |
| [Application: Goto()](https://wiki.asterisk.org/wiki/display/AST/Application_Goto) | Control call flow across priorities and contexts. |
| [Least Cost Routing (LCR) basics](https://www.voip-info.org/least-cost-routing/) | Concepts and strategies for trunk selection. |
| [Number Normalization (E.164)](https://www.voip-info.org/e-164/) | Why to normalize and how to handle country codes. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Asterisk Dialplan Pattern Matching” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+dialplan+pattern+matching) |
| “Inbound vs Outbound Routing in Asterisk/FreePBX” | ~7–12 min | [YouTube](https://www.youtube.com/results?search_query=inbound+outbound+routing+Asterisk) |
| “Least Cost Routing Explained” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=least+cost+routing+VoIP) |

## Deliverable
- Files: `day9_inbound_outbound.conf` with one LCR failover example
- Notes: Pattern rationale, normalization to E.164, and a brief failover test log
- Goal: Reliable inbound and outbound call routing with clear, maintainable patterns.

---

## ✅ Quiz — Day 9 (10 Questions + Answers)

1) What does the pattern `_9NXXNXXXXXX` match?
   - Answer: 9 followed by a 10-digit NANP number (1 digit N + 2 digit XX + 7 digit XXXXXXX).

2) Where would you implement Least Cost Routing (LCR)?
   - Answer: In outbound dialplan logic choosing among multiple trunks.

3) What’s a common reason to normalize numbers to E.164?
   - Answer: Provider requirements and consistency across routes and carriers.

4) Give an example failover strategy when a trunk fails.
   - Answer: Attempt trunk_backup if `DIALSTATUS` not `ANSWER` after trunk_primary.

5) Why are overlapping/ambiguous patterns risky?
   - Answer: They cause unexpected matches; specific patterns should precede broader ones.

6) How can you block international calls by default?
   - Answer: Omit matching patterns or explicitly reject, allow only known prefixes.

7) What should inbound routes typically match?
   - Answer: The DID format delivered by the carrier (e.g., E.164) mapped to contexts.

8) Name a place to send inbound calls before an agent.
   - Answer: IVR or queue context.

9) What is a route loop and how to avoid it?
   - Answer: Goto cycles among contexts without termination; ensure end states and conditions.

10) What should you log in a failover test?
   - Answer: Initial route, failure reason/status code, alternate trunk attempt, and final result.

## Appendix — Deep Dives

### Deep Dive: Dialplan Pattern Strategy and Normalization

- What it is and why it matters: Clear, non‑overlapping patterns prevent misroutes and simplify maintenance. Normalizing to E.164 avoids carrier rejections and eases LCR.
- Key details:
  - Order matters: put specific patterns before broad catch‑alls (e.g., `_911` before `_X.`). [Asterisk Patterns](https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching)
  - Normalize on boundaries: convert user input to E.164 at dial time; map inbound DID format to internal canonical form. [E.164 — ITU](https://www.itu.int/rec/T-REC-E.164/en)
  - Guard rails: explicit exceptions for emergency, service codes, and international prefixes.
- Practical checklist:
  - Add `_911`, `_988` ahead of general patterns.
  - Enforce `+E.164` on outbound; document inbound DID mapping.
  - Include test numbers and a dry‑run context for validation.
- References: [Asterisk Pattern Matching](https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching), [E.164 — ITU](https://www.itu.int/rec/T-REC-E.164/en)

### Deep Dive: Least Cost Routing (LCR) and Failover

- What it is and why it matters: LCR selects the cheapest/most reliable trunk by prefix/time/quality, reducing costs and improving success rates.
- Key details:
  - Selection criteria: prefix tables, time of day, ASR/ACD quality metrics.
  - Failover: conditional retries based on `DIALSTATUS`/SIP code; avoid loops and excessive retry storms.
  - Number formatting: ensure each trunk’s expected format (often E.164) to avoid 403/404. [voip-info LCR](https://www.voip-info.org/least-cost-routing/)
- Practical checklist:
  - Maintain prefix tables per trunk; add cost/priority.
  - Implement bounded retries and backoff.
  - Log per‑trunk attempts with reason codes for audits.
- References: [Least Cost Routing](https://www.voip-info.org/least-cost-routing/)

### Deep Dive: Context Design and Route Isolation

- What it is and why it matters: Separating `from-trunk`, `inbound`, and `outbound` contexts prevents accidental hairpins and enforces security policies.
- Key details:
  - Inbound vs internal: never allow external sources to enter internal/outbound without checks.
  - Hairpin prevention: ensure DID mapping terminates or goes through IVR/queue; avoid Goto back into `from-trunk`.
  - Hinting/presence: use hints in dedicated contexts to reduce coupling.
- Practical checklist:
  - Define minimal privileges per context and document allowed transitions.
  - Add explicit terminals (Hangup, Busy) for unmatched paths.
  - Unit‑test with sample DIDs and internal extensions.
- References: [Asterisk Extensions & Contexts](https://wiki.asterisk.org/wiki/display/AST/Extensions+and+Contexts)

---

## ✅ Quiz — Day 9 (Deep Dives, 10 Questions + Answers)

1) Why place `_911` before `_X.` in a dialplan?
   - Answer: To ensure specific emergency routes match before broad catch‑alls.

2) What canonical number format reduces carrier rejections?
   - Answer: +E.164.

3) Name two LCR selection criteria beyond prefix.
   - Answer: Time of day and quality metrics (ASR/ACD).

4) How do you prevent retry storms on trunk failover?
   - Answer: Bound retries and add backoff/termination conditions.

5) What context should a carrier deliver calls into?
   - Answer: A restricted `from-trunk` context.

6) What is a hairpin route?
   - Answer: A call loop that re‑enters external routing unintentionally.

7) Which SIP outcomes should you log during failover?
   - Answer: SIP status codes and `DIALSTATUS` values.

8) Why segregate inbound and outbound contexts?
   - Answer: Security and policy isolation.

9) What format mismatch often causes 403/404 from carriers?
   - Answer: Non‑E.164 dialing or wrong trunk‑specific formatting.

10) What helps validate dialplan safely before go‑live?
   - Answer: A dry‑run/test context with logging only.

