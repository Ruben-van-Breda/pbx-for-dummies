# Zoiper Extension Walkthrough

This guide shows how to configure an Asterisk test extension and register Zoiper so you can place a call into the demo dialplan.

## Minimal Config Files

### `/etc/asterisk/asterisk.conf`
```ini
[options]
internal_timing = yes

[directories]
astetcdir       => /etc/asterisk
astmoddir       => /usr/lib/asterisk/modules
astvarlibdir    => /var/lib/asterisk
astrundir       => /var/run/asterisk
astlogdir       => /var/log/asterisk
astspooldir     => /var/spool/asterisk

[files]
astctlowner     = asterisk
astctlgroup     = asterisk
astctlmode      = 0660
```

### `/etc/asterisk/pjsip.conf`
```ini
; Replace password to taste. The external_* entries expose your public IP.
[transport-udp]
type                       = transport
protocol                   = udp
bind                       = 0.0.0.0:5060
external_signaling_address = 142.93.232.41
external_media_address     = 142.93.232.41

[6001]
type       = endpoint
transport  = transport-udp
context    = internal
disallow   = all
allow      = ulaw
auth       = 6001
aors       = 6001

[6001]
type       = auth
auth_type  = userpass
password   = SuperSecret6001
username   = 6001

[6001]
type         = aor
max_contacts = 1
```

### `/etc/asterisk/extensions.conf`
```asterisk
[internal]
exten => 6001,1,Answer()
  same => n,Playback(demo-congrats)
  same => n,Hangup()
```

Reload everything when you finish editing:\
`asterisk -rx "pjsip reload"` and `asterisk -rx "dialplan reload"`.

## 1. Prepare the Extension
1. Edit `/etc/asterisk/pjsip.conf` (or `sip.conf` if still using chan_sip).
2. Add an endpoint, auth, and aor for Zoiper:
   ```ini
   [6001]
   type = endpoint
   context = internal
   disallow = all
   allow = ulaw
   auth = 6001
   aors = 6001

   [6001]
   type = auth
   auth_type = userpass
   password = SuperSecret6001
   username = 6001

   [6001]
   type = aor
   max_contacts = 1
   ```
3. Reload the channel driver: `asterisk -rx "pjsip reload"`.

## 2. Update the Dialplan
1. Open `/etc/asterisk/extensions.conf`.
2. Make sure the `internal` context contains a target to call, e.g.:
   ```asterisk
   [internal]
   exten => 6001,1,Answer()
     same => n,Playback(demo-congrats)
     same => n,Hangup()
   ```
3. Reload the dialplan: `asterisk -rx "dialplan reload"`.

## 3. Configure Zoiper
1. Launch Zoiper → `Create new account`.
2. Choose `SIP` and enter:
   - User / Login: `6001`
   - Password: `SuperSecret6001`
   - Domain / Host: IP or DNS of the Asterisk server (e.g., `192.168.1.10`).
3. Skip the wizard’s auto-detect; select `Manual` if prompted.
4. Under `Account → Advanced`, set:
   - Authentication username: `6001`
   - Outbound proxy: leave blank unless NAT requires it.
   - Transport: `UDP` (match your Asterisk endpoint).

## 4. Test the Call
1. Confirm registration from the Asterisk CLI: `asterisk -rx "pjsip show registrations"` and look for `6001`.
2. In Zoiper, dial `6001`. You should hear the demo playback confirming the dialplan path.
3. Watch the CLI (`asterisk -rvvv`) for live tracing. If calls fail, check:
   - Incorrect password → re-enter credentials in Zoiper.
   - NAT/firewall → ensure UDP 5060/5061 and RTP range (default 10000-20000) are open.

## 5. Optional: Add a Second Extension
1. Duplicate the blocks above for `6002`, change passwords, and reload.
2. Call between Zoiper softphones or to a hardware phone registered with the same context.

With these steps, Zoiper can originate calls into your Asterisk lab while staying aligned with the demo dialplan included in this repository.
