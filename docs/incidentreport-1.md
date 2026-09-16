## Incident Report 1 — SSH Brute Force Attack
Date: 16/09/26
Severity: Medium

Summary:
A brute force attack was simulated against the victim VM (192.168.64.4) from the manager VM (192.168.64.5) using 
Hydra with a custom password wordlist targeting the SSH service.

Detection:
Wazuh detected 46 authentication failures within a short timeframe, automatically categorising the activity under 
MITRE ATT&CK techniques: T1110 Brute Force and T1078 Valid Accounts.

Evidence:
- 770+ total alerts generated
- 46+ authentication failures flagged
- Activity visible in Threat Hunting dashboard

Response:
In a real environment the recommended response would be:
- Block the source IP via firewall rule
- Force password reset on targeted account
- Review auth logs for any successful logins during attack window
- Consider implementing fail2ban to auto-block repeated failures
