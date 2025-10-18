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

## Deliverable
- Working IVR with options 1, 2, and operator (0) → voicemail fallback.
