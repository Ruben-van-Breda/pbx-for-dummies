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
