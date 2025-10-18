# Day 7 — Lab 1: Install Your First PBX

## Objectives
- Install Asterisk in Docker or a VM
- Register two softphones and make a call

## Prereqs
- Docker installed (or a Linux VM)
- Two softphones (Zoiper, Linphone)

## Steps (20–30 min)
1) Start Asterisk container:
```
docker run -d --name asterisk --network host ghcr.io/asterisk/asterisk:latest
```
- On macOS without host networking, map ports instead and adjust configs.

2) Create users in `pjsip.conf` (in the container at `/etc/asterisk/pjsip.conf`):
```
; transport
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0

; endpoints
[1000]
type=endpoint
aors=1000
auth=1000
context=internal
allow=ulaw,alaw,opus

[1000]
type=auth
auth_type=userpass
username=1000
password=CHANGEME1000

[1000]
type=aor
max_contacts=1

[1001]
type=endpoint
aors=1001
auth=1001
context=internal
allow=ulaw,alaw,opus

[1001]
type=auth
auth_type=userpass
username=1001
password=CHANGEME1001

[1001]
type=aor
max_contacts=1
```

3) Add dialplan in `extensions.conf`:
```
[internal]
exten => 1000,1,Dial(PJSIP/1000)
exten => 1001,1,Dial(PJSIP/1001)
```

4) Reload Asterisk:
```
asterisk -rvvvvv
pjsip reload
dialplan reload
```

5) Register softphones to 1000 and 1001 (point to PBX IP, UDP 5060)
6) Place a call 1000 ↔ 1001 and verify audio both ways

## Troubleshooting
- See registrations: `pjsip show registrations`, `pjsip show aor 1000`
- See endpoints: `pjsip show endpoints`
- Enable SIP debug: `pjsip set logger on`

## 📚 Further Reading & References

| Resource | Description |
|---|---|
| [Asterisk Docker Image](https://github.com/asterisk/asterisk/tree/master/contrib/docker) | Official container resources and guidance. |
| [Asterisk PJSIP Configuration](https://wiki.asterisk.org/wiki/display/AST/PJSIP+Configuration) | Endpoint/AOR/Auth/Transport reference. |
| [Asterisk Dialplan Basics](https://wiki.asterisk.org/wiki/display/AST/Asterisk+Dialplan) | Core dialplan concepts and examples. |
| [Zoiper Setup](https://www.zoiper.com/en/support/home/article/38/Zoiper3+configuration+%28SIP-IAX%29) | Configure Zoiper with SIP accounts. |
| [Linphone Desktop](https://www.linphone.org) | Open-source softphone downloads and docs. |

### 🎥 Recommended Videos (Free & Short)

| Video | Length | Link |
|---|---|---|
| “Install Asterisk in Docker” | ~6–10 min | [YouTube](https://www.youtube.com/results?search_query=Install+Asterisk+in+Docker) |
| “Asterisk PJSIP Users in 10 Minutes” | ~8–12 min | [YouTube](https://www.youtube.com/results?search_query=Asterisk+PJSIP+users) |
| “Zoiper/Linphone Setup Quickstart” | ~5–8 min | [YouTube](https://www.youtube.com/results?search_query=Zoiper+Linphone+SIP+setup) |

## Deliverable
- Files: `day7_users_pjsip.conf` (user sections), `day7_extensions.conf` (internal context)
- Screenshot: Both softphones registered; Asterisk CLI showing call setup and teardown
- Goal: A working lab PBX with two extensions placing a successful call.
