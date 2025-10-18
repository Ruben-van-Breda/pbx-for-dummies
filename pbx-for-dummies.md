# 📞 PBX & VoIP Fundamentals — 2-Week Course (30 min/day)

A focused, hands-on learning plan to understand **Private Branch Exchanges (PBX)**, **SIP**, **VoIP**, and the modern telecommunication ecosystem.

---

## 🗓️ Overview

**Duration:** 14 Days (30 minutes/day)  
**Goal:** Learn PBX architecture, VoIP signaling, SIP, and real-world call handling  
**Outcome:** Set up and manage a simple working PBX (Asterisk) and understand modern cloud telephony systems

---
<video controls src="Demystifying_the_PBX.mp4" title="Title"></video>
## 📘 Week 1 — PBX & Telephony Foundations

<details>
<summary><b>Day 1 – The Big Picture: What is a PBX?</b></summary>

### 🎯 Objectives
- Understand the role of a PBX in telephony systems  
- Differentiate between PSTN, PBX, and IP-PBX  
- Learn how VoIP disrupted traditional PBXs

### 🧠 Key Concepts
- **PBX** = Private Branch Exchange (internal phone network)  
- **PSTN** = Public Switched Telephone Network  
- **IP-PBX** = Software PBX using Internet Protocol

### ✅ Tasks
- [ ] Sketch a basic PBX topology (phones → PBX → PSTN)  
- [ ] Read: “What is a PBX?” on VoIP-Info.org  

See the expanded lesson: [Week 1 Day 1](./w1/day-01.md)

</details>

---

<details>
<summary><b>Day 2 – Telephony 101: From Analog to Digital</b></summary>

### 🎯 Objectives
- Understand analog/digital lines and signaling  
- Explore FXS/FXO, T1/E1, and trunking concepts  

### 🧠 Key Concepts
- FXS (station) / FXO (office) interfaces  
- Digital signaling: TDM, PRI  
- Call setup vs voice transport

### ✅ Tasks
- [ ] Watch 10-min YouTube: “Analog vs VoIP Explained”  
- [ ] Draw an analog trunk diagram  

See the expanded lesson: [Week 1 Day 2](./w1/day-02.md)

</details>

---

<details>
<summary><b>Day 3 – VoIP Basics: Codecs, Packets & Media</b></summary>

### 🎯 Objectives
- Learn how voice is digitized and transmitted  
- Explore RTP/RTCP and codec negotiation  

### 🧠 Key Concepts
- Codecs: G.711, Opus, G.729  
- RTP = media stream, RTCP = control info  
- Jitter, latency, packet loss

### ✅ Tasks
- [ ] Capture a VoIP call in Wireshark and inspect RTP  

See the expanded lesson: [Week 1 Day 3](./w1/day-03.md)

</details>

---

<details>
<summary><b>Day 4 – SIP Signaling Deep Dive</b></summary>

### 🎯 Objectives
- Learn SIP message types: INVITE, ACK, BYE, REGISTER  
- Read SIP call flows  

### 🧠 Key Concepts
- SIP = Session Initiation Protocol  
- Dialog states, Via/Contact headers  
- User agents and proxy servers  

### ✅ Tasks
- [ ] Read RFC 3261 sections 1–10 (skim)  
- [ ] Trace a SIP call in Wireshark  

See the expanded lesson: [Week 1 Day 4](./w1/day-04.md)

</details>

---

<details>
<summary><b>Day 5 – Inside a PBX: Call Routing & Logic</b></summary>

### 🎯 Objectives
- Understand dialplans and call context  
- Learn how PBX bridges two SIP dialogs  

### 🧠 Key Concepts
- Registrar Server  
- Proxy/B2BUA  
- Dialplan extensions  

### ✅ Tasks
- [ ] Review an example Asterisk dialplan file (`extensions.conf`)  

See the expanded lesson: [Week 1 Day 5](./w1/day-05.md)

</details>

---

<details>
<summary><b>Day 6 – Common PBX Platforms</b></summary>

### 🎯 Objectives
- Compare Asterisk, FreeSWITCH, 3CX, and cloud PBXs  
- Learn about licensing and open-source models  

### 🧠 Key Concepts
- Softswitches and call controllers  
- API-driven PBXs (Twilio, Plivo, Siperb)  

### ✅ Tasks
- [ ] Read: “Asterisk vs FreeSWITCH comparison”  

See the expanded lesson: [Week 1 Day 6](./w1/day-06.md)

</details>

---

<details>
<summary><b>Day 7 – 🧪 Lab 1: Install Your First PBX</b></summary>

### 🎯 Objectives
- Install Asterisk in Docker or a local VM  
- Register two softphones and make a call  

### ✅ Tasks
- [ ] Install Docker and run `docker run -t --name asterisk ghcr.io/asterisk/asterisk`  
- [ ] Configure two SIP users in `pjsip.conf`  
- [ ] Test call between extensions 1000 and 1001  

See the expanded lesson: [Week 1 Day 7](./w1/day-07.md)

</details>

---

## 📗 Week 2 — Modern VoIP & Advanced Concepts

<details>
<summary><b>Day 8 – NAT Traversal: STUN, TURN, ICE</b></summary>

### 🎯 Objectives
- Understand why VoIP fails behind NAT  
- Learn how ICE negotiates media paths  

### 🧠 Key Concepts
- STUN = discover public IP  
- TURN = relay media  
- ICE = Interactive Connectivity Establishment  

### ✅ Tasks
- [ ] Diagram an ICE candidate exchange  

See the expanded lesson: [Week 2 Day 8](./w2/day-08.md)
</details>

---

<details>
<summary><b>Day 9 – Call Routing & Dialplans</b></summary>

### 🎯 Objectives
- Write simple routing rules for inbound/outbound calls  
- Handle extensions and voicemail  

### ✅ Tasks
- [ ] Add an outbound route in Asterisk (`_9XXXX. => Dial(SIP/trunk/${EXTEN:1})`)  

See the expanded lesson: [Week 2 Day 9](./w2/day-09.md)
</details>

---

<details>
<summary><b>Day 10 – SIP Trunking & Carriers</b></summary>

### 🎯 Objectives
- Learn how SIP trunks connect PBXs to the PSTN  
- Understand DID, E.164, and carrier routes  

### ✅ Tasks
- [ ] Configure a demo SIP trunk (e.g., Twilio Elastic SIP)  

See the expanded lesson: [Week 2 Day 10](./w2/day-10.md)
</details>

---

<details>
<summary><b>Day 11 – Security & SBCs</b></summary>

### 🎯 Objectives
- Secure SIP using TLS and SRTP  
- Understand the role of Session Border Controllers  

### ✅ Tasks
- [ ] Enable TLS on Asterisk  
- [ ] Draw an SBC topology  

See the expanded lesson: [Week 2 Day 11](./w2/day-11.md)
</details>

---

<details>
<summary><b>Day 12 – PBX Features & IVRs</b></summary>

### 🎯 Objectives
- Explore voicemail, queues, and auto-attendants  
- Create a basic IVR menu  

### ✅ Tasks
- [ ] Add voicemail for user 1000  
- [ ] Build a simple “Press 1 for Sales” IVR  

See the expanded lesson: [Week 2 Day 12](./w2/day-12.md)
</details>

---

<details>
<summary><b>Day 13 – Monitoring & Troubleshooting</b></summary>

### 🎯 Objectives
- Analyze SIP/RTP traffic  
- Generate and read CDRs  

### ✅ Tasks
- [ ] Use `sip set debug on` in Asterisk CLI  
- [ ] Open a call in Wireshark → Telephony → VoIP Calls  

See the expanded lesson: [Week 2 Day 13](./w2/day-13.md)
</details>

---

<details>
<summary><b>Day 14 – PBX in 2025: Cloud, UCaaS & WebRTC</b></summary>

### 🎯 Objectives
- Understand modern PBX evolution  
- Compare on-prem vs cloud solutions  

### 🧠 Key Concepts
- UCaaS, CPaaS, SBC-as-a-Service  
- WebRTC integration  
- Siperb, Zoom, Twilio Voice API  

### ✅ Tasks
- [ ] Research: “How PBXs integrate with WebRTC and SIP.js”  

See the expanded lesson: [Week 2 Day 14](./w2/day-14.md)
</details>

---

## 🧰 Tools & Resources

| Category | Recommendation |
|-----------|----------------|
| **PBX Software** | [Asterisk](https://www.asterisk.org), [FreeSWITCH](https://freeswitch.com) |
| **Softphones** | [Zoiper](https://www.zoiper.com), [Linphone](https://www.linphone.org) |
| **Packet Analysis** | [Wireshark](https://www.wireshark.org) |
| **Learning Material** | *Asterisk: The Definitive Guide* (4th Ed), SIP RFC 3261 |
| **Videos** | “VoIP Call Flow Explained” (NetworkChuck, YouTube) |

---

## 🧩 Final Outcome

By the end of this course, you’ll:
- ✔ Explain how SIP signaling and RTP media work  
- ✔ Install and configure an Asterisk PBX  
- ✔ Write dialplans for inbound/outbound routing  
- ✔ Integrate a SIP trunk and test external calls  
- ✔ Trace and debug SIP traffic  
- ✔ Understand modern PBX evolution into UCaaS and WebRTC

---

> 💡 **Tip:**  
> Extend this course into a third week focused on *WebRTC + SIP.js integration* or *building a simple call center using Asterisk queues*.