# Day 10 — SIP Trunking & Carriers

## Objectives
- Understand SIP trunks to PSTN
- Configure a demo trunk and test calls

## Concepts (Explained)
- DID: Direct Inward Dial number mapped to your PBX
- E.164: International numbering format (+<country><number>)
- Registration vs IP-ACL trunks: PBX registers to provider vs provider allows your IP

## Steps (20–30 min)
1) Choose a provider trial (e.g., Twilio Elastic SIP, ClearlyIP, etc.)
2) Configure trunk in `pjsip.conf`:
```
[trunk_primary]
type=endpoint
transport=transport-udp
aors=trunk_primary
auth=trunk_primary
outbound_auth=trunk_primary
context=from-trunk
allow=ulaw,alaw

[trunk_primary]
type=auth
auth_type=userpass
username=YOUR_SIP_USERNAME
password=YOUR_SIP_PASSWORD

[trunk_primary]
type=aor
contact=sip:PROVIDER_SIP_DOMAIN_OR_IP
```
3) Provider side: Point DID to your PBX public IP:5060 (or to your registrar user)
4) Inbound: Map DID in `from-trunk` to IVR or an extension
5) Outbound: Add dial pattern to use the trunk (from Day 9)

## Validation
- `pjsip show registrations`
- Make inbound and outbound test calls
- Check CLI for SIP 4xx/5xx and audio path

## Quick Check (Self-test)
- Difference between registration and IP-based trunking?
- Why normalize numbers to E.164?

## Common Pitfalls
- Auth vs IP trunks: Using registration on a provider expecting static IP (or vice versa) leads to 403/404 failures.
- NAT and SIP ALG: One-way/no audio or failed registration; disable SIP ALG and set correct external IP/ports.
- Codec mismatch/transcoding: Provider only supports G.711; avoid unnecessary transcoding and confirm codec lists.
- Number formatting: Provider requires E.164 (+1...), normalize outbound and match inbound DID format exactly.
- Firewall/RTP ranges: Open SIP (UDP 5060/5061) and the provider’s RTP UDP range; confirm media path.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Twilio Elastic SIP Trunking](https://www.twilio.com/voice/sip-trunking) | Example provider docs and onboarding. |
| [Plivo SIP Trunking](https://www.plivo.com/sip-trunking/) | Alternative CPaaS trunk. |
| [E.164 Numbering — ITU](https://www.itu.int/rec/T-REC-E.164/en) | Official numbering plan reference. |
| [Caller ID and Number Formatting](https://www.voip-info.org/callerid/) | Considerations for CLI/CNAM and formatting. |
| [Asterisk PJSIP Trunk Examples](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Trunk+Configuration) | Reference patterns and options. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Set up a SIP Trunk (Asterisk/FreePBX)” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=setup+SIP+trunk+Asterisk) |
| “E.164 Explained” | ~5–8 min | [YouTube](https://www.youtube.com/results?search_query=E.164+explained) |
| “Troubleshooting SIP Trunks” | ~7–12 min | [YouTube](https://www.youtube.com/results?search_query=troubleshooting+SIP+trunk) |

## Deliverable
- Files: `day10_trunk_pjsip.conf`, `day10_inbound_outbound_tests.md`
- Notes: Provider chosen, registration/IP-ACL mode, normalized dial patterns, and results of inbound/outbound tests
- Goal: A functioning trunk with clear dialing and verified call flow end-to-end.
