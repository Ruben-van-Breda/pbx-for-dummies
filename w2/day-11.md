# Day 11 — Security & SBCs

## Objectives
- Secure SIP with TLS and media with SRTP (lab)
- Understand Session Border Controllers (SBCs)

## Concepts (Explained)
- TLS for SIP: Encrypts signaling; requires certificates and `transport=tls`
- SRTP: Encrypts media; negotiated via SDP (e.g., `a=crypto` or DTLS-SRTP)
- SBC: B2BUA focused on security, topology hiding, NAT traversal, QoS, and policy enforcement

## Lab (20–30 min)
1) Generate self-signed certs (lab only)
2) Enable TLS transport in `pjsip.conf`:
```
[transport-tls]
type=transport
protocol=tls
bind=0.0.0.0:5061
method=tlsv1_2
cert_file=/etc/asterisk/keys/asterisk.pem
priv_key_file=/etc/asterisk/keys/asterisk.key
```
3) Allow SRTP on endpoints:
```
media_encryption=sdes
```
4) Configure softphones to use TLS/SRTP
5) Verify in CLI and Wireshark (no plain SIP; RTP not decodable)

## Notes
- In production prefer proper PKI and possibly DTLS-SRTP
- Consider an SBC for internet-facing trunks and remote users

## Quick Check (Self-test)
- Why doesn’t TLS alone secure audio?
- What benefits does an SBC add beyond TLS/SRTP?

## Common Pitfalls
- Certificate issues: Wrong CN/SAN or expired cert → softphones reject TLS; match FQDN and time-sync servers.
- TLS versions/ciphers: Client requires TLS 1.2/1.3; align method/cipher suites on both ends.
- SIPS vs SIP over TLS: Some clients expect `sips:` scheme; ensure transport and URIs match.
- SRTP mode mismatch: Endpoint uses DTLS-SRTP while PBX expects SDES (or vice versa) → no media.
- NAT over TLS: 5061 blocked or wrong external address; open firewall and set correct external signaling.
- SBC headers: Topology hiding or header normalization can break auth; whitelist headers and test.

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [RFC 5246/8446 — TLS 1.2/1.3](https://www.rfc-editor.org/rfc/rfc8446) | Transport Layer Security protocol specs (1.3). |
| [RFC 3711 — SRTP](https://www.rfc-editor.org/rfc/rfc3711) | Secure RTP specification. |
| [DTLS-SRTP — RFC 5764](https://www.rfc-editor.org/rfc/rfc5764) | How SRTP keys are negotiated over DTLS. |
| [Asterisk Secure Calling (TLS/SRTP)](https://wiki.asterisk.org/wiki/display/AST/Secure+Calling) | Asterisk-specific setup and considerations. |
| [What is an SBC? — Vendor-neutral explainer](https://www.voip-info.org/sbc/) | Roles SBCs play in VoIP deployments. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “TLS vs SRTP Explained” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=TLS+vs+SRTP+explained) |
| “Secure SIP with TLS/SRTP in Asterisk” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+TLS+SRTP+setup) |
| “What is an SBC?” | ~5–8 min | [YouTube](https://www.youtube.com/results?search_query=What+is+an+SBC+VoIP) |

## Deliverable
- Files: `day11_pjsip_tls.conf` (transport and endpoint snippets)
- Screenshot: Wireshark showing TLS handshake and no decodable RTP payload
- Goal: Demonstrate encrypted signaling and media in a simple lab setup.

---

## ✅ Quiz — Day 11 (10 Questions + Answers)

1) What does TLS secure in SIP and what port is commonly used?
   - Answer: Signaling (not media) over TCP; commonly 5061 for TLS.

2) How is media encrypted in VoIP?
   - Answer: SRTP (often negotiated via SDES or DTLS-SRTP in SDP).

3) Name two roles of an SBC.
   - Answer: Topology hiding/NAT traversal and policy/QoS/security enforcement.

4) Why doesn't TLS alone secure audio?
   - Answer: It encrypts signaling only; media flows over RTP unless SRTP is used.

5) What two files are referenced for TLS keys in the example transport?
   - Answer: `asterisk.pem` (cert) and `asterisk.key` (private key).

6) Which SRTP mode did the example enable on endpoints?
   - Answer: SDES (`media_encryption=sdes`).

7) Give one Wireshark indicator that SRTP is active.
   - Answer: RTP payload not decodable; only packet sizes/timing visible.

8) A softphone rejects TLS due to CN/SAN mismatch. Fix?
   - Answer: Use a cert with correct FQDN in SAN and ensure time sync.

9) What TLS versions are typically required by modern clients?
   - Answer: TLS 1.2 or 1.3.

10) When should you consider an SBC?
   - Answer: For internet-facing trunks/remote users to handle security, NAT, and policy.
