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

## Further Reading
- Dialplan patterns — `https://wiki.asterisk.org/wiki/display/AST/Pattern+Matching`

## Deliverable
- A draft inbound/outbound dialplan with one backup trunk.
