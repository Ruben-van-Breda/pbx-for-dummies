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

## Appendix — Deep Dives

### Deep Dive: TLS Ciphers, Versions, and Certificate Hygiene

- What it is and why it matters: Correct TLS versions/ciphers and valid certificates prevent handshake failures and MITM risks.
- Key details:
  - Prefer TLS 1.2/1.3; disable outdated versions/ciphers. [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446)
  - Certificates must include correct SANs (FQDN) and valid chains; clients validate hostname.
  - Align client/server cipher suites; test with `openssl s_client`.
- Practical checklist:
  - Use modern cipher suites; renew certs before expiry; sync time (NTP).
  - Verify CN/SAN match and intermediate chain on the server.
- References: [RFC 8446 — TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446), [Asterisk Secure Calling](https://wiki.asterisk.org/wiki/display/AST/Secure+Calling)

### Deep Dive: SRTP Keying — SDES vs DTLS‑SRTP

- What it is and why it matters: SRTP protects media; key exchange method impacts security and interoperability.
- Key details:
  - SDES: Keys signaled in SDP (`a=crypto`); simpler but exposes keys to signaling path.
  - DTLS‑SRTP: Keys negotiated over DTLS; preferred in WebRTC and modern endpoints. [RFC 5764](https://www.rfc-editor.org/rfc/rfc5764)
  - Interop: Ensure both ends support the same mode; otherwise expect no media.
- Practical checklist:
  - Prefer DTLS‑SRTP where possible; fall back to SDES for legacy.
  - Confirm cipher suites (AES_CM_128_HMAC_SHA1_80) and rekey behavior.
- References: [RFC 3711 — SRTP](https://www.rfc-editor.org/rfc/rfc3711), [RFC 5764 — DTLS‑SRTP](https://www.rfc-editor.org/rfc/rfc5764)

### Deep Dive: SBC Capabilities and Topology Hiding

- What it is and why it matters: SBCs act as B2BUAs that enforce security policies, normalize signaling, and hide internal topology.
- Key details:
  - Header manipulation: remove private IPs, normalize `Via`/`Contact`, enforce auth.
  - Media anchoring: centralize RTP, enable transcoding and QoS; simplifies NAT traversal.
  - Policy and DoS controls: rate limiting, access lists, TLS termination.
- Practical checklist:
  - Place SBC at the edge; define ingress/egress policies; log SIP and media metrics.
  - Test with encrypted and legacy peers; verify failover.
- References: [voip-info SBC](https://www.voip-info.org/sbc/)

---

## ✅ Quiz — Day 11 (Deep Dives, 10 Questions + Answers)

1) Which TLS version should be preferred today?
   - Answer: TLS 1.2 or 1.3 (prefer 1.3 where supported).

2) What SAN requirement often breaks TLS for SIP clients?
   - Answer: Missing/correct FQDN in the certificate SAN.

3) A security drawback of SDES vs DTLS‑SRTP?
   - Answer: Keys travel in SDP signaling with SDES.

4) Which SRTP cipher is widely supported?
   - Answer: AES_CM_128_HMAC_SHA1_80.

5) Name two SBC functions that help NAT traversal.
   - Answer: Media anchoring and header normalization/topology hiding.

6) How to test TLS handshake and ciphers from CLI?
   - Answer: `openssl s_client -connect host:5061 -tls1_2` (or `-tls1_3`).

7) What indicates DTLS‑SRTP is negotiated?
   - Answer: DTLS handshake followed by SRTP keys; no `a=crypto` lines.

8) Why is time sync important for TLS?
   - Answer: Certificate validity checks rely on accurate time.

9) Where should an SBC be placed?
   - Answer: At the network edge between providers/Internet and your PBX.

10) What breaks media when SRTP modes mismatch?
   - Answer: One end uses SDES while the other expects DTLS‑SRTP.

