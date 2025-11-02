# Asterisk Hands-On: Core CLI, PJSIP Driver, and Real-World Commands

This guide complements the introductory material by diving into the commands and configuration files you’ll use when running an IP-PBX on Asterisk with the modern **PJSIP** stack. It assumes you already have Asterisk installed and can reach the CLI (`sudo asterisk -rvvv`).

---

## 1. CLI Power Moves

These commands keep you in control of the PBX runtime.

| Command | What it does | When to use |
| --- | --- | --- |
| `core show help` | Lists all CLI commands | First stop when you forget syntax |
| `core show version` | Asterisk build info & modules | Verify features and security patches |
| `core set verbose 5` | Raises console verbosity | Troubleshoot live call flows |
| `core show channels` | Active call legs | Confirm bridges, identify leaks |
| `core show applications` | Available dialplan apps | Discover playback, queue, voicemail etc. |
| `module show like pjsip` | Module load status | Ensure PJSIP stack is active |
| `module reload` | Reloads dialplan + modules | Apply config changes without restart |
| `dialplan show` | Dumps compiled dialplan | Confirm pattern matching & contexts |
| `logger show channels` | Shows logging backends | Make sure logs land in files/syslog |

> Tip: If you script repetitive diagnostics, wrap CLI commands in `sudo asterisk -rx "<command>"`.

---

## 2. From chan_sip to PJSIP: Architectural Shift

- **chan_sip** is the legacy monolithic SIP driver. It’s single-instance, harder to secure, and deprecated.
- **PJSIP** is the modular replacement built on the PJSIP library. It separates transport, authentication, and endpoint identity, making multihoming, TLS/SRTP, and scaling patterns cleaner.

Key objects in `pjsip.conf`:

| Section | Purpose |
| --- | --- |
| `[transport-udp]` (`type=transport`) | Defines IPs/ports and NAT hints for UDP/TCP/TLS |
| `[6001]` (`type=endpoint`) | SIP UA capabilities: codecs, transports, context |
| `[6001]` (`type=auth`) | Credentials: username/password, MD5, etc. |
| `[6001]` (`type=aor`) | Contact locations (registrations or static host) |

An endpoint references its `aor` & `auth`, and optionally inbound/outbound `identify` rules.

---

## 3. Minimal PJSIP Configuration

`/etc/asterisk/pjsip.conf`:

```ini
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0:5060

[6001]
type=endpoint
context=internal
disallow=all
allow=ulaw
auth=6001
aors=6001
rtp_symmetric=yes
force_rport=yes
rewrite_contact=yes

[6001]
type=auth
auth_type=userpass
username=6001
password=SuperSecretPass

[6001]
type=aor
max_contacts=1
```

Pair with dialplan logic in `/etc/asterisk/extensions.conf`:

```ini
[internal]
exten => 6001,1,Answer()
 same => n,Playback(hello-world)
 same => n,Hangup()
```

Reload after edits:

```bash
sudo asterisk -rx "module reload res_pjsip.so res_pjsip_endpoint_identifier_user.so"
sudo asterisk -rx "dialplan reload"
```

---

## 4. Inspecting PJSIP State

- `pjsip show endpoints` — all registered endpoints with codec lists and states.
- `pjsip show endpoint 6001` — deep dive: transport, auth, contacts, media settings.
- `pjsip show contacts` — current bindings (IP:port) for registered devices.
- `pjsip show registrations` — outbound registrations from Asterisk to ITSPs.
- `pjsip show transports` — confirm listen IP/ports, TLS cert files.
- `pjsip list aors` — confirm max contacts, support for multiple devices per extension.

For signaling debug:

```bash
sudo asterisk -rx "pjsip set logger on"   # Mirrors SIP messages to the CLI/logs
sudo asterisk -rx "pjsip set logger off"
```

---

## 5. Live Call Troubleshooting Flow

1. **Check registrations:** `pjsip show registrations`, `pjsip show contacts`.
2. **Watch signaling:** `pjsip set logger on` and re-create the call—observe INVITE/200/ACK.
3. **Inspect RTP:** Confirm `rtp set debug on` (Asterisk CLI) to see media IP/port. Use Wireshark if one-way audio persists.
4. **Dialplan logic:** `dialplan show <context>` ensures patterns route as expected; validate `same => n` expansions.
5. **Media negotiation:** In `pjsip show endpoint <id>`, confirm `allow=` includes the desired codecs (Opus, G.711, etc.).
6. **Security:** For TLS/SRTP issues, verify certificates in the transport and `media_encryption=sdes` or `dtls`.

---

## 6. Automating with the CLI

- **Call origination:** `channel originate PJSIP/6001 extension 6002@internal` — quickly test ring paths.
- **Presence / hints:** `core show hints` — confirm BLF states if using `exten => 6001,hint,PJSIP/6001`.
- **Voicemail:** `voicemail show users` — check mailbox provisioning.
- **CDR/CEL:** `cdr show` or inspect database/file outputs depending on `cdr.conf`.

Use shell scripts or Ansible to push changes, then run a reload:

```bash
#!/usr/bin/env bash
set -euo pipefail
sudo install -m 640 my_pjsip.conf /etc/asterisk/pjsip.d/custom.conf
sudo asterisk -rx "module reload res_pjsip.so"
```

---

## 7. When Things Break

- **401/407 auth failures:** Revisit `username`/`password` in both the endpoint and the phone/ITSP. Use `pjsip show registrations verbose` to display error codes.
- **No audio:** Confirm RTP flows with `rtp debug`. Enable `rtp_symmetric`, `force_rport`, `rewrite_contact` on endpoints behind NAT.
- **TLS handshake errors:** Check `pjsip show transports` for cert paths; inspect `/var/log/asterisk/messages`.
- **Module conflicts:** Make sure `chan_sip.so` is **not** loaded (comment it out in `modules.conf`) if you only want PJSIP handling UDP/5060.

---

## 8. Useful References

- `man pjsip.conf` — inline documentation on every option.
- [Asterisk Wiki — PJSIP Configuration Examples](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration+Examples)
- [PJSIP Stack Documentation](https://www.pjsip.org/docs.htm)
- `res_pjsip.conf.sample` within `/etc/asterisk/samples/` for advanced features (endpoint groups, auth ACLs, outbound registration templates).

With these commands and patterns you can confidently manage SIP signaling, register trunks, harden security, and triage calls directly from the Asterisk CLI.

