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

## Deliverable
- Working trunk config + successful inbound and outbound call test notes.
