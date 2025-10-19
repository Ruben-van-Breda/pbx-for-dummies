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

---

## ✅ Quiz — Day 10 (10 Questions + Answers)

1) What is a DID?
   - Answer: A Direct Inward Dial number mapped to your PBX for inbound calls.

2) What is E.164 formatting?
   - Answer: International format: `+<countrycode><number>`.

3) Difference between registration and IP-ACL trunks?
   - Answer: Registration authenticates with credentials; IP-ACL allows traffic from specified IPs.

4) Where do you map an inbound DID in Asterisk?
   - Answer: In a context like `from-trunk` routing to IVR/extension.

5) Why normalize outbound numbers?
   - Answer: To match provider expectations (E.164) and ensure routing success.

6) Name two common causes of trunk failures seen in CLI.
   - Answer: NAT/SIP ALG issues and number formatting errors (4xx/5xx responses).

7) Which codecs are safe defaults for many providers?
   - Answer: G.711 (ulaw/alaw).

8) What command checks registration status?
   - Answer: `pjsip show registrations`.

9) What’s a good outbound test after trunk config?
   - Answer: Place a call using normalized pattern and check audio both ways.

10) Why open provider RTP ranges on firewall?
   - Answer: So media can flow; SIP may work otherwise but audio will fail.

## Appendix — Deep Dives

### Deep Dive: Registration vs IP‑Based Trunks — Security and Reliability

- What it is and why it matters: Choosing between credentialed registration and IP‑ACL trunks affects reachability, security posture, and failover.
- Key details:
  - Registration: NAT‑friendly for dynamic IPs; requires auth; provider learns your current contact URI.
  - IP‑ACL: Static IPs with firewall rules; lower signaling overhead but brittle if IP changes.
  - Security: Lock registration to TLS and strong creds; lock IP‑ACL to exact source IPs and ports.
- Practical checklist:
  - Confirm provider mode; avoid mixing auth on IP‑ACL trunks.
  - Use TLS for signaling and open only provider RTP ranges.
  - Monitor registration refreshes and 401/403 failures.
- References: [Asterisk PJSIP Trunk Config](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Trunk+Configuration)

### Deep Dive: Number Normalization and Caller ID (CLI/CNAM)

- What it is and why it matters: Consistent E.164 formatting and proper caller identity increase answer rates and reduce rejections.
- Key details:
  - Outbound formatting: enforce `+E.164`; map local dialing to international.
  - CLI/CNAM: Providers may require verified caller IDs; stamping `From`/`P-Asserted-Identity` differs by trunk. [voip-info CallerID](https://www.voip-info.org/callerid/)
  - Inbound DID mapping: normalize to internal format for routing.
- Practical checklist:
  - Add normalization functions; document per‑trunk CLI rules.
  - Test with multiple carriers for interop.
- References: [ITU E.164](https://www.itu.int/rec/T-REC-E.164/en)

### Deep Dive: NAT/Media Considerations for Trunks

- What it is and why it matters: Even with successful SIP signaling, media often fails due to NAT/firewalls; correct external addresses and ports are critical.
- Key details:
  - Disable SIP ALG; set PBX external signaling/media addresses.
  - Align codecs (G.711 safe default) and confirm RTP port ranges are open.
  - Consider SBCs or media anchoring when endpoints/carriers are behind NAT.
- Practical checklist:
  - Verify SDP contains correct public IP; run a test call and check RTP both ways.
  - Open provider RTP UDP range; confirm no double‑NAT issues.
- References: [Asterisk NAT Support](https://wiki.asterisk.org/wiki/display/AST/Asterisk+SIP+NAT+support)

---

## ✅ Quiz — Day 10 (Deep Dives, 10 Questions + Answers)

1) Why might you choose registration over IP‑ACL?
   - Answer: Dynamic IPs/NAT friendliness and provider‑learned contact URI.

2) A risk unique to IP‑ACL trunks?
   - Answer: Breakage on IP changes or asymmetric routing.

3) Which header may be required to present a verified caller ID?
   - Answer: `P-Asserted-Identity` (varies by provider).

4) What default codec is widely accepted by carriers?
   - Answer: G.711 (ulaw/alaw).

5) What NAT feature should usually be disabled on firewalls?
   - Answer: SIP ALG.

6) Where should E.164 normalization happen?
   - Answer: At the outbound routing boundary.

7) What indicates a registration auth issue?
   - Answer: 401/403 responses in CLI.

8) What confirms media is flowing bidirectionally?
   - Answer: RTP seen both ways (ports/RTCP stats) and two‑way audio.

9) Why anchor media via an SBC?
   - Answer: Normalize NAT and ensure routable RTP paths.

10) What to validate after enabling TLS on trunks?
   - Answer: Certificate CN/SAN, method (TLS1.2/1.3), and provider compatibility.

