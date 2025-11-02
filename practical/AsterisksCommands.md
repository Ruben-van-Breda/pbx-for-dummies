Asterisks CLI
```
asterisk -rvvv
```

List all PJSIP endpoints (your “extensions”):
```
sudo asterisk -rx "pjsip show endpoints"
```
Details for one endpoint:
```
sudo asterisk -rx "pjsip show endpoint 6001"
```
See registration/contacts:
```
sudo asterisk -rx "pjsip show contacts"
sudo asterisk -rx "pjsip show aors"
sudo asterisk -rx "pjsip show registrations"   # outbound regs
```
Dialplan extensions (not SIP endpoints):
```
sudo asterisk -rx "dialplan show"
sudo asterisk -rx "dialplan show from-internal"
```

## Appendix — Deep Dives

### Deep Dive: PJSIP Core Objects (Endpoint, AOR, Auth, Contact)

- Why it matters: Understanding PJSIP’s building blocks makes it easier to read CLI output and debug registration/calling issues.
- Key details:
  - Endpoint: Device/user profile (codecs, transport, context). References an AOR and optional Auth.
  - AOR (Address of Record): One or more reachable Contacts for an identity; controls registration behavior (e.g., `max_contacts`).
  - Auth: Credentials policy used for inbound auth (REGISTER/INVITE) and/or outbound registration.
  - Contact: Runtime address registered to an AOR (learned via REGISTER or statically defined).
  - CLI mapping: `pjsip show endpoints` (summary), `pjsip show endpoint 6001` (details), `pjsip show aors`, `pjsip show contacts`.
- Example (minimal pjsip.conf):
```
[6001](endpoint)
type=endpoint
context=from-internal
aors=6001
auth=6001

[6001](aor)
type=aor
max_contacts=1

[6001](auth)
type=auth
auth_type=userpass
username=6001
password=supersecret
```
- References: [PJSIP Configuration](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration), [PJSIP Configuration Examples](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration+Examples)

### Deep Dive: Asterisk CLI — Interactive vs Remote, Verbosity, and SIP Logging

- Why it matters: Choosing the right invocation and verbosity speeds up troubleshooting and enables scripting.
- Key details:
  - `asterisk -r` attaches to a running CLI; add `-vvv` to start with higher verbosity. Adjust live with `core set verbose 3` and `core set debug 0..9`.
  - `asterisk -rx "<command>"` executes a single command non-interactively (ideal for scripts/cron). Output is returned to stdout.
  - Enable SIP message tracing with `pjsip set logger on` (disable with `off`). Use selectively on production.
  - Many CLI commands depend on loaded modules; use `module show like pjsip` if a command is missing.
- Example:
```
asterisk -rvvv
asterisk -rx "pjsip show endpoints"
*CLI> core set verbose 4
*CLI> pjsip set logger on
```
- References: [Command Line Interface](https://wiki.asterisk.org/wiki/display/AST/Command+Line+Interface), [PJSIP Logging](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Logging)

### Deep Dive: Dialplan Contexts vs SIP Endpoints

- Why it matters: Dialplan extensions are call-flow rules, not SIP devices; mapping the two prevents routing mistakes.
- Key details:
  - A context groups extensions and controls call permissions; endpoints enter the dialplan in their `context` (e.g., `from-internal`).
  - An extension is a pattern with ordered priorities (`1, n, ...`) executing applications (e.g., `Dial()`).
  - To call a device, dial its endpoint/technology, not its defined extension unless you intentionally route it.
  - Inspect with `dialplan show` or `dialplan show from-internal`.
- Example (minimal extensions.conf):
```
[from-internal]
exten => 6001,1,NoOp(Call endpoint 6001)
 same => n,Dial(PJSIP/6001)
 same => n,Hangup()
```
- References: [Asterisk Dialplan](https://wiki.asterisk.org/wiki/display/AST/Asterisk+Dialplan), [Contexts, Extensions, and Priorities](https://wiki.asterisk.org/wiki/display/AST/Contexts%2C+Extensions%2C+and+Priorities)