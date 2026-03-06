# Windows Logging for SOC – Incident Investigation

## Investigation Overview

This investigation was conducted in a simulated SOC environment to analyze suspicious activity on a Windows system using Windows Event Logs, Sysmon telemetry, and PowerShell history.  

The objective was to identify malicious behavior, determine how an attacker gained access, and analyze the actions taken after the initial compromise.

The investigation focused on authentication events, account creation activity, malicious file downloads, persistence mechanisms, and command-and-control communication.

---

## Environment

Lab Platform: TryHackMe  
Operating System: Windows  
Monitoring Tools:

- Windows Event Viewer
- Sysmon
- PowerShell command history
- VirusTotal for hash analysis

Primary log sources analyzed:

- Windows Security Logs
- Sysmon Operational Logs
- PowerShell history

---

## Initial Suspicious Activity – Brute Force Attack

The investigation began by analyzing Windows Security logs in Event Viewer.

Security logs contain valuable authentication events that allow analysts to detect suspicious login activity.

Important authentication event IDs include:

Event ID 4624 – Successful login  
Event ID 4625 – Failed login attempt  

While reviewing the logs, a large number of failed login attempts (Event ID 4625) were observed originating from the same IP address.

Source IP Address:
```
10.10.53.248
```

The repeated authentication failures suggested a **brute force attack attempt**.

Shortly after the failed login attempts, a successful login event (Event ID 4624) was recorded from the same IP address using the **Administrator account**, indicating the attacker successfully gained access to the system.

---

## Attacker Account Creation

After gaining administrative access, the attacker created a new user account to maintain access.

By reviewing account management events in the security logs, the following activity was identified.

Event ID 4720 – User account created  
Event ID 4732 – User added to a security-enabled group  

Analysis of these logs revealed that the attacker created a new account:

```
svc-sysrestore
```

The attacker then added this account to the following group:

```
Remote Desktop Users
```

This action allowed the attacker to maintain remote access to the system even if the administrator account was secured later.

---

## Malware Download Investigation

Further investigation moved into Sysmon logs to analyze endpoint activity.

Using Sysmon's file creation events, suspicious activity associated with a user named **Sarah** was identified.

Filtering for **Sysmon Event ID 11 (FileCreate)** revealed that Sarah downloaded an executable file from the web browser.

Downloaded file:

```
ckjg.exe
```

Sysmon logs also recorded a file stream creation event containing the **file hash**.

The hash was submitted to **:contentReference[oaicite:0]{index=0}**, where the file was identified as **malicious** based on multiple antivirus detections.

This confirmed that the downloaded file was malware.

---

## Persistence Mechanism

Further analysis identified the creation of a persistence mechanism designed to execute malicious code automatically when the system starts.

A suspicious startup file was discovered at the following location:

```
C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url
```

Placing a file in the Windows Startup folder allows malware to automatically execute whenever the user logs into the system.

This indicates the attacker attempted to maintain persistent access to the compromised machine.

---

## Command and Control Communication

Network activity associated with the malicious process revealed outbound connections to a command-and-control (C2) server.

Suspicious connection:

```
193.46.217.4:7777
```

The IP address resolved to the domain:

```
hkfasfsafg.click
```

This communication suggests the malware was attempting to receive commands or transmit data to an external attacker-controlled server.

---

## PowerShell Activity

Reviewing the PowerShell command history revealed additional attacker actions performed on the compromised system.

The attacker accessed and removed a file named:

```
notes.txt
```

This indicates the attacker used PowerShell to interact with system files after gaining access.

PowerShell activity is commonly used by attackers for post-exploitation actions due to its powerful administrative capabilities.

---

## Attack Summary

The investigation revealed the following attack chain:

1. Brute force attack against the Administrator account from IP **10.10.53.248**
2. Successful login using the Administrator account
3. Creation of a persistent backdoor user **svc-sysrestore**
4. Addition of the backdoor account to the **Remote Desktop Users** group
5. Malware download (**ckjg.exe**) through a web browser
6. Malware persistence created through a startup folder file
7. Command-and-control communication with **193.46.217.4:7777**
8. Additional attacker activity observed through PowerShell history

---

## MITRE ATT&CK Mapping

The activity observed during this investigation maps to several techniques within the :contentReference[oaicite:1]{index=1} framework.

Brute Force Authentication  
T1110

Create Account  
T1136

Persistence via Startup Folder  
T1547

Command and Control Communication  
T1071

PowerShell Execution  
T1059

---

## Conclusion

This investigation demonstrated how Windows event logs and Sysmon telemetry can be used to detect malicious activity and reconstruct an attacker’s actions on a compromised system.

By analyzing authentication logs, account management events, file creation activity, and network connections, it was possible to identify the full attack chain from initial access to persistence and command-and-control communication.

Understanding how to analyze these logs is critical for SOC analysts when investigating security incidents and detecting attacker behavior on Windows systems.

---

## Skills Demonstrated

Windows Event Log Analysis  
Sysmon Log Investigation  
Authentication Log Monitoring  
Malware Detection Using File Hash Analysis  
Persistence Detection  
Command and Control Identification  
Threat Investigation Methodology
