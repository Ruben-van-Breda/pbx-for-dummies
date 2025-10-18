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
