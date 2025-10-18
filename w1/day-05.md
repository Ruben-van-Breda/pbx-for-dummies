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

## Further Reading
- Asterisk Dialplan — `https://wiki.asterisk.org/wiki/display/AST/Dialplan`

## Deliverable
- A short draft dialplan snippet for internal and outbound calls.
