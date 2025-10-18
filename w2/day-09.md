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
