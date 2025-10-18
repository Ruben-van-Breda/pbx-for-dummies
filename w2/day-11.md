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

## Deliverable
- Screenshot of TLS transport active and a successful SRTP call.
