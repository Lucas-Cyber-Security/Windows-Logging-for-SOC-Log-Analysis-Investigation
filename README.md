# Windows Logging for SOC – Security Investigation

This repository contains a simulated security investigation conducted in a Windows environment using Event Viewer, Sysmon, and PowerShell logs. The goal was to detect and analyze malicious activity on an endpoint, reconstruct the attack chain, and identify persistence and command-and-control behavior.

The investigation focuses on:

- Detecting brute-force login attempts and unauthorized access  
- Tracking creation of backdoor accounts and privilege escalation  
- Identifying malicious file downloads and verifying file hashes  
- Analyzing persistence mechanisms through the Startup folder  
- Investigating network communication with potential C2 servers  
- Reviewing PowerShell history for post-exploitation activity  

## Skills Demonstrated

- Windows Event Log Analysis  
- Sysmon Telemetry Investigation  
- Authentication and Account Management Monitoring  
- Malware Detection using File Hash Analysis  
- Persistence and C2 Identification  
- SOC-style Threat Investigation Methodology  
- Mapping attacker behavior to the MITRE ATT&CK framework

## Contents

- `investigation_report.md` – The full SOC-style investigation report  
- `screenshots/` – Evidence captured from logs during the investigation  

---
