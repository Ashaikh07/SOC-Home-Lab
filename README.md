# SOC Home Lab & SIEM Configuration

## Overview
A personal home lab project simulating a Security Operations Centre (SOC) 
environment. Wazuh open-source SIEM is deployed across virtualised machines 
to replicate enterprise security monitoring with real-world attacks simulated 
and documented.

## Project Status

## Progress Log

### Session 1 - 14/09/26
- Created two Ubuntu VMs in UTM (Manager: 192.168.64.3, Victim: 192.168.64.4)
- Configured shared networking, resolved MAC address/IP conflict
- Installed Docker on manager VM
- Deployed Wazuh via Docker compose (containers running)
- Dashboard access in progress - resolving VM display issue

## Lab Architecture
- Wazuh Manager — central SIEM server receiving and processing alerts
- Victim VMs — Windows/Linux machines with Wazuh agents installed
- Attack Machine — used to simulate brute-force and exploitation techniques

## Attacks Simulated
- [ ] SSH brute-force (Hydra)
- [ ] RDP brute-force
- [ ] Basic Metasploit exploitation
- [ ] Suspicious process activity

*(Will be updated with screenshots and findings as the lab progresses)*

## Custom Detection Rules
Wazuh detection rules written to identify simulated attack patterns — 
see `/rules` folder

## Incident Reports
Structured write-ups documenting each attack: what happened, how it was 
detected, and recommended remediation — see `/reports` folder

## Technologies Used
- Wazuh SIEM
- VirtualBox
- Linux (Ubuntu)
- Bash

## References & Learning
- [Wazuh Documentation](https://documentation.wazuh.com)
- [MITRE ATT&CK Framework](https://attack.mitre.org)
- TryHackMe — Pre-Security Path
