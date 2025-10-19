# Day 12 — PBX Features & IVRs

## Objectives
- Enable voicemail for a user
- Build a simple IVR menu

## Concepts (Explained)
- Voicemail context: Mailbox definitions and storage
- IVR (Auto-attendant): Menu that routes callers by DTMF input
- Time conditions: Different behavior based on business hours

## Steps (20–30 min)
1) Configure voicemail (Asterisk `voicemail.conf` example):
```
[default]
1000 => 1234,User 1000,user1000@example.com
```
2) Reference voicemail in dialplan:
```
[inbound]
; fall through to IVR

[ivr-main]
exten => s,1,Answer()
 same => n,Background(custom/welcome)
 same => n,WaitExten(5)

exten => 1,1,Goto(sales,s,1)
exten => 2,1,Goto(support,s,1)
exten => 0,1,Dial(PJSIP/1000,20)
 same => n,Voicemail(1000@default,u)
 same => n,Hangup()

exten => t,1,Playback(vm-goodbye)
 same => n,Hangup()
```
3) Record prompts (or use defaults) and test DTMF

## Quick Check (Self-test)
- What happens on timeout in `WaitExten(5)`?
- Where do voicemail PINs live?

## Common Pitfalls
- DTMF mode mismatch: Endpoint uses in-band while PBX expects RFC 4733 → menu not recognized.
- Prompt recording levels: Too quiet or noisy prompts reduce DTMF success; normalize audio.
- Timeout handling: No `t` or invalid option path → callers get stuck or hang up.
- Mailbox quotas/paths: Voicemail fails to save due to permissions/quota; verify storage backend.
- Time conditions: Missing business-hours logic routes callers to wrong destinations.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Asterisk Voicemail](https://wiki.asterisk.org/wiki/display/AST/Voicemail) | Voicemail configuration, options, storage backends. |
| [Asterisk Applications: Background(), WaitExten(), Voicemail()](https://wiki.asterisk.org/wiki/display/AST/Asterisk+11+Application+List) | Reference for apps used in simple IVRs. |
| [DTMF — RFC 4733](https://www.rfc-editor.org/rfc/rfc4733) | RTP payload for telephone events (RFC 2833/4733). |
| [IVR Design Tips](https://www.voip-info.org/ivr/) | Best practices for simple, usable menus. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Create a Simple IVR in Asterisk” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Create+simple+IVR+in+Asterisk) |
| “Asterisk Voicemail Setup” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+voicemail+setup) |
| “DTMF in VoIP (RFC 2833/4733)” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=DTMF+RFC+2833+4733) |

## Deliverable
- Files: `day12_voicemail.conf`, `day12_ivr_extensions.conf`
- Notes: IVR options, DTMF mode configured, timeout path, and voicemail test results
- Goal: A basic but functional auto-attendant with voicemail fallback.

---

## ✅ Quiz — Day 12 (10 Questions + Answers)

1) Where are voicemail mailboxes defined in Asterisk?
   - Answer: In `voicemail.conf` under a voicemail context (e.g., `[default]`).

2) Which dialplan application plays a prompt and waits for DTMF?
   - Answer: `Background()` (plays prompt) and `WaitExten()` (waits for input).

3) What does the `t` extension handle in an IVR?
   - Answer: Timeout when no DTMF is received.

4) Where do voicemail PINs live?
   - Answer: In the mailbox definition (e.g., `1000 => 1234,...`).

5) Give two common DTMF transmission methods.
   - Answer: In-band audio and RFC 4733 (formerly RFC 2833) telephone events.

6) Why normalize prompt recording levels?
   - Answer: To ensure clear recognition and a consistent caller experience.

7) How would you route to a user if option 0 is pressed?
   - Answer: `exten => 0,1,Dial(PJSIP/1000,20)` with voicemail fallback.

8) What should happen after voicemail is left?
   - Answer: Play a goodbye and hang up; confirm storage path/quota.

9) Why are time conditions important in IVRs?
   - Answer: Different routing after-hours (voicemail, messages) vs business hours.

10) What should be tested when validating IVR DTMF?
   - Answer: Correct option mapping, timeout behavior, and audio levels.

## Appendix — Deep Dives

### Deep Dive: DTMF Transport — In‑Band vs RFC 4733 vs SIP INFO

- What it is and why it matters: IVRs rely on accurate DTMF; mismatched methods cause missed inputs or double‑detection.
- Key details:
  - In‑band: Tones ride in audio; sensitive to compression and noise.
  - RFC 4733 events: Out‑of‑band RTP payload for reliable detection. [RFC 4733](https://www.rfc-editor.org/rfc/rfc4733)
  - SIP INFO: Signaled over SIP; may not pass through all networks/SBCs.
- Practical checklist:
  - Prefer RFC 4733 with non‑lossy codecs; avoid in‑band with compressed codecs.
  - Verify end‑to‑end DTMF method with providers.
- References: [RFC 4733](https://www.rfc-editor.org/rfc/rfc4733)

### Deep Dive: IVR Design — Usability and Timeout Handling

- What it is and why it matters: Simple, fast menus reduce abandonment and errors.
- Key details:
  - Keep menus shallow (≤ 3 options per level); provide 0 for operator or voicemail.
  - Timeouts: implement `t` and invalid (`i`) handlers; replay prompt briefly before exit. [Asterisk Apps](https://wiki.asterisk.org/wiki/display/AST/Asterisk+11+Application+List)
  - Audio: normalize prompts to consistent loudness to improve recognition.
- Practical checklist:
  - Script options clearly; confirm DTMF after selection; log path chosen.
  - Provide after‑hours path via time conditions.
- References: [IVR Best Practices](https://www.voip-info.org/ivr/)

### Deep Dive: Voicemail Storage and Quotas

- What it is and why it matters: Proper storage prevents failures and ensures compliance.
- Key details:
  - Backends: filesystem vs ODBC/IMAP integration; set folder permissions correctly. [Asterisk Voicemail](https://wiki.asterisk.org/wiki/display/AST/Voicemail)
  - Quotas/retention: avoid full mailboxes; set sensible limits and notification.
  - Security: PIN policies; avoid trivial PINs and brute force exposure.
- Practical checklist:
  - Monitor disk usage; rotate/expire old messages.
  - Enforce PIN length and lockout after repeated failures.
- References: [Asterisk Voicemail](https://wiki.asterisk.org/wiki/display/AST/Voicemail)

---

## ✅ Quiz — Day 12 (Deep Dives, 10 Questions + Answers)

1) Which DTMF method is most robust across compressed links?
   - Answer: RFC 4733 telephone events.

2) Why avoid in‑band DTMF with lossy codecs?
   - Answer: Compression distorts tones causing misdetection.

3) What handler should be present for no input in an IVR?
   - Answer: `t` (timeout) handler.

4) One usability guideline for IVR menus?
   - Answer: Keep options shallow and under ~3 per level.

5) Where to store voicemail PINs and settings?
   - Answer: `voicemail.conf` mailbox definitions.

6) A benefit of IMAP/ODBC voicemail backends?
   - Answer: Centralized storage and easier management/replication.

7) How to prevent full mailboxes from breaking delivery?
   - Answer: Set quotas and retention policies with alerts.

8) Security measure for voicemail access?
   - Answer: Enforce strong PINs and lockouts on repeated failures.

9) Why normalize audio prompts?
   - Answer: Improve DTMF recognition and caller experience.

10) What should you log for IVR selections?
   - Answer: Caller path/option chosen and any timeout/invalid events.

