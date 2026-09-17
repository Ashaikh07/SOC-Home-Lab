## Incident Report 2 — Security Configuration Assessment
Date: 17/09/26
Type: Proactive Vulnerability Detection
Severity: Medium

Summary:
Wazuh automatically performed a Security Configuration Assessment (SCA) against the victim VM (192.168.64.4) benchmarking it against 
CIS (Centre for Internet Security) Ubuntu Linux 24.04 LTS standards.

Findings:
The victim VM scored 48/100 on the CIS benchmark — below the 50% threshold — indicating significant security 
misconfigurations on the endpoint.

Key issues identified:
- SSH MaxAuthTries not configured — no limit on login attempts, leaving the system vulnerable to brute force attacks
- SSH MaxSessions not configured — no limit on having simultaneous sessions
- SSH MACs not configured — weak cryptographic algorithms permitted
- SSH LogLevel not configured — insufficient logging for audit trails
- SSH LoginGraceTime not configured — extended window for authentication attempts

Why This Matters:
These misconfigurations directly contributed to the success of the SSH brute force simulation in Incident 1. A properly 
hardened system with MaxAuthTries set to 3 would have significantly slowed down the Hydra attack.

Recommended Remediation:
Edit /etc/ssh/sshd_config on the victim VM and add:
- Set MaxAuthTries to 3 - Locking out anyone who fails logins 3 times
- Set MaxSessions to 3 - Limit simultaneous connections
- Set LoginGraceTime to 30 - A 30 second timeout on long in attempts
- Set LogLevel to VERBOSE - Records more details about who connects
- Specify strong MACs - Allow more modern secure cryptographic algorithms like, hmac-sha2-256

These changes would bring the system closer to CIS complianceand reduce the attack surface significantly.
