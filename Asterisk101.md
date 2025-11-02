## Asterisk 101: A Beginner's Guide
### Part 1: Setting Up Asterisk
#### 1. Installing Asterisk

On a Linux-based system like Ubuntu, you can install Asterisk using the package manager:

```bash
sudo apt-get update
sudo apt-get install asterisk
```

#### 2. Initializing and Starting Asterisk

To start the Asterisk service:

```bash
sudo systemctl start asterisk
```


And to make sure it starts on boot:

```bash
sudo systemctl enable asterisk
```

#### 3. Accessing the Asterisk Console

Connect to the Asterisk CLI with verbose output to see what's going on:

```bash
sudo asterisk -rvvv
```

#### 4. Adding a Simple Dial Plan

Edit your dial plan in `/etc/asterisk/extensions.conf` and add the following under the `[default]` context:

```ini
[default]
exten => 100,1,Answer()
exten => 100,2,Playback(demo-congrats)
exten => 100,3,Hangup()
```

#### 5. Updating and Reloading the Dial Plan

After saving your changes, reload the dial plan from the Asterisk console:

```bash
dialplan reload
```

#### 6. Checking Asterisk Status

To check if Asterisk is running:

```bash
sudo systemctl status asterisk
```


Or inside the Asterisk console:

```bash
core show uptime
```

### Part 2: Connecting Zoiper to Asterisk
#### 1. Setting Up a SIP Extension

In `sip.conf` (or `pjsip.conf`), add a SIP user:

```ini
[100]
type=friend
secret=mysecretpassword
host=dynamic
context=default
```

#### 2. Configuring Zoiper

- Username: Use the extension number (e.g., 100)
- Password: The password you set (e.g., mysecretpassword)
- Domain/Host: The IP address of your Asterisk server (e.g., 192.168.1.10)

Save the settings, and once Zoiper registers, you can dial 100 to test your connection.

## Appendix — Deep Dives

### Deep Dive: SIP vs PJSIP in Asterisk

- What it is and why it matters: Asterisk supports two SIP stacks: legacy `chan_sip` and modern `res_pjsip` (PJSIP). PJSIP is the recommended, actively developed stack offering better NAT handling, modular objects, and improved interoperability.
- Key details:
  - PJSIP uses distinct objects in `pjsip.conf`: `transport`, `endpoint`, `aor`, and `auth`. This separation simplifies management and scaling.
  - Prefer PJSIP for new deployments; `chan_sip` is in maintenance mode. Do not load both for the same peers to avoid conflicts.
  - Useful CLI: `pjsip show endpoints`, `pjsip show contacts`, `module show like pjsip`.
  - Interop: Explicitly set codecs (`disallow=all`, `allow=ulaw,alaw`) and a `transport` to avoid surprises.
- Practical example:

```ini
; pjsip.conf (minimal single extension)
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0

[100]
type=endpoint
transport=transport-udp
aors=100
auth=100
context=default
disallow=all
allow=ulaw

[100]
type=auth
auth_type=userpass
username=100
password=mysecretpassword

[100]
type=aor
max_contacts=1
```

- References: [PJSIP Configuration (Asterisk Wiki)](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration), [PJSIP Configuration Examples (Asterisk Wiki)](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration+Examples), [RFC 3261 — SIP (IETF, 2002)](https://www.rfc-editor.org/rfc/rfc3261)

### Deep Dive: Dialplan Fundamentals

- What it is and why it matters: The dialplan (`extensions.conf`) is Asterisk’s call routing logic. Understanding contexts, extensions, priorities, and pattern matching is essential to build reliable call flows.
- Key details:
  - Contexts isolate call domains; use `include => othercontext` to share rules.
  - Priorities start at 1; use `same => n` for readability and labels for jumps.
  - Pattern matching: `_NXXNXXXXXX` (N=2–9, X=0–9), `.` and `!` for variable-length, and character classes like `[123]`.
  - Useful CLI: `dialplan show`, `core show applications`, `core show functions`.
- Practical example:

```ini
; extensions.conf
[internal]
exten => 100,1,Answer()
 same => n,Playback(demo-congrats)
 same => n,Hangup()

; 10-digit NANP pattern to trunk/provider endpoint
exten => _NXXNXXXXXX,1,Dial(PJSIP/${EXTEN}@provider,30)
 same => n,Hangup()

[default]
include => internal
```

- References: [Dialplan (Asterisk Wiki)](https://wiki.asterisk.org/wiki/display/AST/Dialplan), [Asterisk Dialplan Examples (Asterisk Wiki)](https://wiki.asterisk.org/wiki/display/AST/Asterisk+Dialplan+Examples)

### Deep Dive: NAT Traversal for SIP/RTP

- What it is and why it matters: NAT can break SIP signaling and RTP media, causing one‑way audio or failed calls. Correctly configuring PJSIP and transports avoids these issues.
- Key details:
  - On `transport`: set your public IPs and local nets so Asterisk advertises reachable addresses.
  - On `endpoint`: enable NAT helpers like `rtp_symmetric=yes`, `rewrite_contact=yes`, `force_rport=yes`; consider `direct_media=no` when endpoints are behind NAT.
  - Prefer endpoints that support ICE; enable `ice_support=yes` for better traversal.
  - Disable SIP ALG on routers/firewalls when possible.
- Practical example:

```ini
; pjsip.conf (NAT-aware)
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0
external_signaling_address=203.0.113.10
external_signaling_port=5060
external_media_address=203.0.113.10
local_net=192.168.1.0/24

[100]
type=endpoint
transport=transport-udp
aors=100
auth=100
context=default
disallow=all
allow=ulaw
rtp_symmetric=yes
rewrite_contact=yes
force_rport=yes
ice_support=yes
direct_media=no
```

- References: [NAT Configuration (Asterisk Wiki)](https://wiki.asterisk.org/wiki/display/AST/NAT+Configuration), [RFC 5389 — STUN (IETF, 2008)](https://www.rfc-editor.org/rfc/rfc5389), [RFC 8445 — ICE (IETF, 2018)](https://www.rfc-editor.org/rfc/rfc8445)