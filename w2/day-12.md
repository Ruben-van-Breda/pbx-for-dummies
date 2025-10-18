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
