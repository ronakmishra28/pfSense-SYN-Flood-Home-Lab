# pfSense Firewall Home Lab  
**Blocking Real SYN Flood Attacks with Custom Firewall Rules**

Hands-on lab demonstrating simulation and blocking of SYN flood attacks using pfSense custom firewall rules.

## Overview

I built this lab to gain real hands-on experience with configuring and defending a production-grade firewall — the kind of work SOC analysts and network security engineers do every day.

I installed pfSense from scratch on my ASUS Windows laptop using VirtualBox and created a realistic dual-network environment. I then launched a SYN flood attack from Kali Linux, watched it overwhelm the firewall, and finally blocked it with custom rules.

---

## Lab Setup

- **Host Machine**: ASUS Windows laptop + VirtualBox
- **Firewall**: pfSense 2.8.1
- **Attacker VM**: Kali Linux (WAN side) – IP 10.0.2.15
- **Management VM**: Ubuntu (LAN side) – IP 192.168.1.11
- **pfSense WAN Gateway**: 10.0.2.3
- **pfSense LAN Gateway**: 192.168.1.1
- **Networking**: Bridged adapter on both VMs

---

## Table of Contents
- [Initial Setup](#initial-setup)
- [Network Configuration](#network-configuration)
- [Custom Firewall Rules](#custom-firewall-rules)
- [Attack Simulation](#attack-simulation)
- [Blocking the Attack](#blocking-the-attack)
- [Challenges & Troubleshooting](#challenges)

---

## Initial Setup

I downloaded the pfSense ISO and created a new VM in VirtualBox. After booting, I completed the text-based console setup wizard (hostname, domain, WAN interface) and reset the default admin password.

![pfSense initial console setup](screenshots/pfsense%20setup%20screen.png)

![Resetting default admin password](screenshots/resetting%20the%20password.png)

## Network Configuration

I configured the WAN interface and enabled the DHCP server on the LAN side so the Ubuntu machine could get an IP automatically.

![WAN Interface Configuration](screenshots/wan%20settings.png)

![Unlocking WAN access](screenshots/unlocking%20access%20for%20private%20network%20-%20wan.png)

![DHCP confirmed](screenshots/dhcp%20confirmed.png)

![Ping successful before rules](screenshots/ping%20successful.png)

## Custom Firewall Rules

I created two rules on the WAN interface: one to block the Kali attacker IP and another to catch SYN flood attempts. I enabled logging on both.

![Creating source IP block rule](screenshots/firewall%20rules%20setting%20source%20ip%20address.png)

![Block rule for hping3](screenshots/setting%20block%20rules%20for%20hping.png)

![Both rules active](screenshots/both%20of%20them%20applied.png)

## Attack Simulation

From Kali Linux I ran:

```bash
hping3 -S -p 80 --flood --rand-source 10.0.2.3
