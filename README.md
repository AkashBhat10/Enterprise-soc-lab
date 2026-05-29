# 🛡️ Enterprise SOC Lab — Red, Blue & Purple Teaming

**Tools Used:** Kali Linux · pfSense · Snort IDS/IPS · Wireshark · Ettercap · Zphisher · CCProxy · Active Directory · VirtualBox  
**Environment:** Virtualised lab simulating a real-world small business network  
**Role:** Team Leader (Session 4) — Final Implementation & Documentation  

---

## Overview

This project involved designing and implementing a cybersecurity management plan for a simulated business client operating a cloud-based digital content subscription platform. The engagement covered the full red team, blue team, and purple team cycle — attacking, defending, and combining findings to harden the environment.

I led the final implementation session, was responsible for the phishing defence implementation, co-implemented the Wi-Fi encryption fix, and contributed to blue team activities across all attack scenarios.

---

## Environment Setup

- **Virtualisation:** VirtualBox hosting Windows 10 and Kali Linux VMs
- **Firewall/IDS/IPS:** pfSense with Snort configured for real-time detection and blocking
- **Attacker Machine:** Kali Linux (dedicated)
- **Network:** Segmented virtual network simulating internal and guest Wi-Fi zones
- **Active Directory:** Windows Server configured for Group Policy enforcement

---

## Vulnerabilities Identified & Mitigated

### 1. ARP Poisoning
**What:** Identified that the network had no protection against ARP spoofing, allowing a man-in-the-middle attack to redirect all traffic through the attacker's machine.  
**How:** Used Ettercap on Kali Linux to successfully redirect traffic destined for the pfSense gateway to the attacker machine. Verified interception using Wireshark.  
**Result:** Mitigated by implementing static ARP entries via Active Directory Group Policy across all workstations, eliminating the attack vector. Post-fix Wireshark capture confirmed zero ARP poisoning activity.

---

### 2. Guest Wi-Fi Session Hijacking
**What:** Discovered that the guest Wi-Fi network was completely open with no encryption, allowing an attacker to capture session cookies and hijack authenticated web sessions.  
**How:** Connected to the unencrypted access point, used Wireshark to capture HTTP traffic and extract session cookies from a live browser session, then injected the cookie to gain authenticated access to the victim's account.  
**Result:** Fixed by enforcing WPA2-PSK encryption on the access point and routing all guest traffic through HTTPS-only filtering. Post-fix Wireshark capture confirmed all traffic was encrypted and no cookies were extractable.

---

### 3. SYN Flood (DoS Attack)
**What:** Identified that the network had no protection against volumetric denial-of-service attacks targeting the server.  
**How:** Executed a SYN flood attack from Kali Linux, continuously sending SYN packets without completing the handshake to exhaust server resources.  
**Result:** Deployed a custom Snort rule on pfSense to detect SYN flood patterns (threshold: 20 SYN packets to the same destination within 60 seconds). Snort alerts fired correctly and pfSense automatically blocked the malicious source IPs. Verified through Snort alert logs.
