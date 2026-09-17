# SOC Home Lab Documentation

## 1. Lab Architecture
- **Manager VM:** Ubuntu Server 24.04 ARM64, 4GB RAM, IP 192.168.64.5
- **Victim VM:** Ubuntu Desktop 24.04 ARM64, 2GB RAM, IP 192.168.64.4
- **Host Machine:** MacBook M3, 8GB RAM, UTM hypervisor
- **Wazuh Version:** 4.14.7 (deployed via Docker Compose)

## 2. Setup Log
[14/09/26] — Created two Ubuntu VMs in UTM,
[14/09/26] — Configured shared network, resolved IP conflict,
[14/09/26] — Attempted Wazuh quickstart install (failed, see issues),
[14/09/26] — Switched to Docker deployment,
[15/09/26] — Created fresh Ubuntu Server VM, installed Docker,
[15/09/26] — Deployed Wazuh via Docker Compose successfully,
[15/09/26] — Dashboard accessible at https://192.168.64.5,
[16/09/26] — Installed Wazuh agent on victim VM,
[16/09/26] — Victim VM appearing as active agent in dashboard,
[16/09/26] — Simulated SSH brute force attack using Hydra,
[16/09/26] — 770+ alerts generated, 46+ authentication failures detected,
[16/09/26] — MITRE ATT&CK technique identified: T1110 Brute Force,
[17/09/26] - Simulated Nmap Reconnaissance Scan using Nmap,
[17/09/26] - OS identified and Port 22 identified to be running OpenSSH,
[17/09/26] - Wazuh SCA triggered CIS assessment returning a security score,
[17/09/26] - MITRE ATT&CK technique identified: T1595 Active Scanning

## 3. Wazuh Credentials
URL: https://192.168.64.5

## 4. Attacks Simulated

**Attack 1: SSH Brute Force**
Date: 16/09/26
Tool: Hydra
Target: 192.168.64.4 (victim VM) port 22
Command: hydra -l user -P passwords.txt ssh://192.168.64.4 -t 4
Result: 770 total alerts, 46 authentication failures detected
MITRE Technique: T1110 - Brute Force, T1078 - Valid Accounts
See: incidentreport-1.md

**Attack 2: Nmap Reconnaissance Scan**
Date: 17/09/26
Tool: Nmap
Target: 192.168.64.4
Command: sudo nmap -sV -O 192.168.64.4
Result: Port 22 identified as open running OpenSSH 9.6p1, OS identified as Linux. 
Wazuh SCA automatically triggered CIS benchmark assessment revealing 48/100 security score.
MITRE Technique: T1595 Active Scanning, T1046 Network Service Discovery
See: incidentreport-2.md

## 5. Detection Rules Written
[in progress — custom rules to be added]

## 6. Incident Reports
- Incident 1: SSH Brute Force Attack (16/09/26)
- Incdient 2: Nmap Reconnaissance Scan (17/09/26)

## 7. Issues & How I Fixed Them

**Issue 1: Duplicate IP addresses on cloned VMs**
Date: 14/09/26
Problem: After cloning the Ubuntu VM to create the manager VM both VMs were assigned the same IP address meaning they couldn't communicate independently on the network.
Cause: The cloned VM inherited the same MAC address as the original causing DHCP to assign identical IPs to both machines.
Fix: Regenerated a new MAC address on the manager VM through UTM network settings. On restart DHCP assigned a unique IP.
Result: Both VMs now have distinct IPs on the same shared network and can communicate with each other and the host Mac.

**Issue 2: Wazuh install script version incompatibility**
Date: 14/09/26
Problem: Attempted Wazuh install with version 4.9, failed with "incompatible system" error despite aarch64 architecture.
Fix: Updated to correct current version 4.14. Install proceeded.

**Issue 3: Wazuh installation failed with "No space left on device"**
Date: 14/09/26
Problem: Install failed during wazuh-dashboard package unpacking despite 16GB showing as free on main partition. Temporary files from the install process filled a smaller partition.
Fix: Ran apt-get clean, autoremove and cleared /tmp. Reran install.

**Issue 4: Wazuh install script repeatedly failing on ARM64**
Date: 14/09/26
Problem: wazuh-install.sh failed multiple times — space issues, port conflicts from partial install, then wazuh-keystore binary not found preventing manager from starting.
Fix: Abandoned script-based install. Switched to Docker deployment which has official ARM64 image support.

**Issue 5: Wazuh dashboard returning 500 error — disk full**
Date: 15/09/26
Problem: All Docker containers showing as Up but dashboard returning Internal Server Error. Logs showed TimeoutError connecting to OpenSearch indexer.
Root cause: 30GB disk completely full — Docker images (15.89GB) + containers (8.6GB) + volumes (7.4GB) = 32GB on a 30GB disk.
Fix: Resized VM disk from 30GB to 70GB in UTM, ran growpart and lvextend to expand the Linux partition. Dashboard loaded successfully after restart.

**Issue 6: SSH not starting on victim VM**
Date: 16/09/26
Problem: SSH service failing to start on victim VM, preventing Hydra brute force simulation.
Cause: Missing SSH host keys on the cloned VM.
Fix: Ran sudo ssh-keygen -A to regenerate all missing host keys. SSH service started successfully.

## 8. Next Steps
- [x] Deploy Wazuh SIEM via Docker
- [x] Connect victim VM as monitored agent
- [x] Simulate SSH brute force attack
- [x] Document first incident report
- [x] Simulate Nmap reconnaissance scan
- [ ] Write custom Wazuh detection rule
- [ ] Simulate Metasploit exploitation
- [ ] Document further incident reports
