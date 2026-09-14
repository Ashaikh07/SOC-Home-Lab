# SOC Home Lab Documentation

## 1. Lab Architecture
- Manager VM: Ubuntu, 4GB RAM, IP 192.168.64.3
- Victim VM: Ubuntu, 2GB RAM, IP 192.168.64.4
- Host Machine: MacBook M3, 8GB RAM, UTM hypervisor

## 2. Setup Log
[14/09/26] — Created two Ubuntu VMs in UTM
[14/09/26] — Configured shared network, resolved IP conflict
[14/09/26] — Attempted Wazuh quickstart install (failed, see issues)
[14/09/26] — Switched to Docker deployment
[14/09/26] — Wazuh containers running, dashboard access in progress

## 3. Wazuh Credentials
URL: https://192.168.64.3
Username: *****
Password: *****

## 4. Attacks Simulated
[to be completed]

## 5. Detection Rules Written
[to be completed]

## 6. Incident Reports
[to be completed]

## 7. Issues & How I Fixed Them

**Issue 1: Duplicate IP addresses on cloned VMs**
Date: 14/09/26
Problem: After cloning the Ubuntu VM to create the manager VM, both VMs were assigned the same IP address, meaning they couldn't communicate independently on the network.
Cause: The cloned VM inherited the same MAC address as the original causing DHCP to assign identical IPs to both machines.
Fix: Regenerated a new MAC address on the manager VM through the UTM network settings. On restart DHCP assigned a unique IP. 
Result: Both VMs now have distinct IPs on the same shared network and can communicate with each other and the host Mac.

**Issue 2: Wazuh installation failed with "No space left on device"**
Date: 14/09/26
Problem: Install failed during wazuh-dashboard package unpacking despite 16GB showing as free on main partition. Temporary files from the install process filled a smaller partition during extraction.
Fix: Ran apt-get clean, autoremove and cleared /tmp to free up space. Reran install script.

**Issue 4: Wazuh install script repeatedly failing on ARM64**
Date: 14/09/26
Problem: wazuh-install.sh failed multiple times — space issues, port conflicts from partial install, then wazuh-keystore binary not found preventing manager from starting.
Fix: Abandoned script-based install. Switched to Docker deployment which has official ARM64 image support.

**Issue 5: Wazuh dashboard returning 500 error**
Date: 14/09/26
Problem: All Docker containers showing as Up but dashboard returning Internal Server Error. Logs showed TimeoutError connecting to OpenSearch indexer.
Cause: Disk filled to 100% during deployment.
Fix: Resized manager VM disk from 30GB to 64GB in UTM.


## 8. Next Steps
- Confirm Wazuh dashboard accessible at https://192.168.64.3
- Install Wazuh agent on victim VM
- Simulate first brute force attack
- Write first detection rule
- Document first incident report
