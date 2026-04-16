# SOC-BTLO-Labs  
**Blue Team / SOC Analyst Investigation Portfolio**  
This repository showcases my hands-on SOC investigation work based on BTLO (Blue Team Labs Online) scenarios.  
Each case includes a full PDF report with:  
- Incident overview  
- Log & network evidence analysis  
- SIEM queries  
- MITRE ATT&CK mapping  
- IOC extraction  
- Root cause analysis  
- Remediation recommendations  

This portfolio demonstrates real SOC workflows including alert triage, threat hunting, malware analysis, and incident response documentation.

---

## Tools & Techniques Used
- **SIEM Platforms:** Splunk, Elastic  
- **Log Sources:** Sysmon, Windows Event Logs, Web server logs, PCAP  
- **Network Analysis:** Wireshark, Zeek  
- **Forensics:** Autopsy, RegRipper, Strings, Hashing tools  
- **Threat Intelligence:** VirusTotal, AbuseIPDB, ANY.RUN  
- **IR Methodology:** Identification → Containment → Eradication → Recovery → Lessons Learned  
- **Reporting:** Professional SOC-style PDF reports  

---

## Core Skills Demonstrated
- Alert triage & prioritization  
- SIEM query building & log correlation  
- Malware behavior analysis  
- Network traffic investigation  
- IOC extraction & enrichment  
- MITRE ATT&CK mapping  
- Incident timeline reconstruction  
- Root cause analysis  
- Professional SOC report writing  

---

# **Investigation Reports**
---
## 12.**Rekcod**  
**Scenario:** It is easy to casually pull a docker image from docker hub and run it. But can you trust them? What if you need to create a Dockerfile from an image?  
**What I did:**  
- Enumerated all local Docker images and validated the total count.
- Reviewed each image’s metadata using docker history and docker inspect.
- Analyzed filesystem layers to identify created directories, ownership, and permissions.
- Extracted and inspected image layers using docker save + tar to review OS‑level artifacts.
- Parsed JSON configuration files with jq to identify environment variables and layer timestamps.
- Evaluated image efficiency and wasted space using Dive.
- Compared findings across images to assess trustworthiness and structural consistency.  
**Findings:**  
- Total images: 11.
- Redis: Creates /data, owned by redis:redis.
- Fedena index.html: /usr/share/doc/adduser/examples/adduser.local.conf.examples/skel.other/index.html, Permissions: 644.
- dduportal/bats: OS‑like layer created at 1970‑01‑01T00:00:00Z.
- WordPress CLI: WORDPRESS_CLI_GPG_KEY = 63AF7AA15067C05616FDDD88A3A2E8F226F0BC06
- hhvm/hhvm: Efficiency score 99%, wasted space 9 MB.  
**Tools:** jq, Docker, Dive.  
**Lessons Learned:**  
- Docker commands and flags, including comamnds images, history, inspect, run, save.
- Docker layer structure.

---

## 11.**SAM**  
**Scenario:** User executed a malicious HTA file leading to remote code execution, reverse shell, and credential theft via SAM/SYSTEM hive exfiltration.   
**What I did:**  
- Reviewed PCAP → identified reverse shell from 172.16.0.4 → 172.16.0.5:80
- Analyzed Sysmon logs → confirmed mshta.exe → powershell.exe execution chain
- Decoded Base64 + Gzip payload → identified hta-psh msfvenom stager
- Found UseShellExecute = false indicating execution without OS shell
- Extracted SAM & SYSTEM → recovered 6 NTLM hashes
- Cracked passwords for Admin (Password!) and Sam (StandardUser)
- Memory forensics (netscan) → attacker logged in via port 22
- Identified additional malicious script: jaws-enum.ps1  
**Findings:**  
- Initial access via malicious sample_template.hta
- PowerShell payload decompressed using GzipStream
- Reverse shell established to attacker
- Credential dumping via SAM/SYSTEM
- Attacker escalated and performed further enumeration  
**Tools:** Wireshark, Sysmon, CyberChef, Volatility，Hashes  
**Lessons Learned:**  
- Block mshta usage
- Monitor PowerShell encoded commands
- Protect registry hive permissions
- Enforce strong passwords & disable unused accounts
- Monitor outbound connections to unusual ports

---

## 10. **HASHISH**  
**Scenario:** Confidential Administrator documents were stolen. Invesigate how the attacker abused local privilege escalation and credential dumping to access sensitive files.  
**What I did:**  
- Checked recent files, event logs (access denied).
- Reviewed PowerShell history → found Invoke-HiveNightmare.ps1.
- Identified exploit CVE‑2021‑36934 (HiveNightmare).
- Found dumped registry hives (SAM/SYSTEM/SOFTWARE).
- Used Impacket secretsdump to extract NTLM hashes.
- Used psexec.py + Pass‑the‑Hash to escalate to SYSTEM.
- Located stolen document in Administrator’s Documents folder.
- Identified writable share via net share.  
**Findings:**  
- Local privilege escalation via HiveNightmare.
- NTLM hash extracted → SYSTEM access via psexec.
- Administrator password recovered from stolen file.   
**Tools:** PowerShell, Impacket (secretsdump, psexec)   
**Lessons Learned:**  
- Restrict ACLs on registry hives.
- Monitor PowerShell history + script execution.
- Limit admin shares & enforce RDP hardening.
- Deploy EDR + outbound firewall rules.

---

## 9. **PEAK2**  
**Scenario:** This investigation about the activities of an attacker built reverse webshell on Mountain Top Solutions' Linux server.   
**What I did:**  
- Analyzed server logs(syslog, auth.log, auditd logs, apache logs) to identify malicious requests  
- Traced attacker’s IP, user-agent(Hydra), and exploitation path
- Identified local privilege escalation enumeration.
- Identified unauthorized root SSH log on system.
- Identified unauthorized download and execution payloads.
- Identified Start PHP built-in web sever
- Identified FTP connections with attacker's IP
- Identified file exfiltration.
- Mapped attacker behavior to MITRE (T1082,T1083,T1059,T1083,T1078,T1041,T1105,T1505.003)   
**Findings:**  
- Privilege escalation. 
- Attacker deployed a reverse PHP web shell and exfiltrate creticle files.   
**Tools:** Wireshark，Sublime  
**Lessons Learned:**  
- Restrict SSH to VPN or jump Host
- Deploy EDR monitoring on hosts.
- Implement outbound firewall rules.
- Monitor for reverse connections (FTP, high ports).

---

## 8. **PEAK**  
**Scenario:** Unusual activity originating in the logs on the application development server.    
**What I did:**  
- Analyzed server logs(syslog, auth.log, auditd logs, apache logs) to identify malicious requests  
- Traced attacker’s IP, user-agent(Hydra), and exploitation path
- Identified SSh Brute Force.
- Identified unauthorized download and execution payloads.
- Identified exploitation of Sudo(CVE-2021-3156)
- Identified file exfiltration to ngrok.io.
- Identified Data Destruction.
- Mapped attacker behavior to MITRE (T1110,T1105,T1204,T1083,T1041,T1485)    
**Findings:**  
- Attacker implement malware Hydra to log on system.
- Attacker escalate privilege and upload files to extranet.  
**Tools:** Elatic  
**Lessons Learned:**  
- Enforce MFA for SSH.
- Restrict SSH to VPN or jump host.
- Patch sudo to latest version.
- Deploy EDR monitoring on Linux hosts

---

## 7. **Defaced (Enterprise)**  
**Scenario:** Corporate web server defacement incident.  
**What I did:**  
- Analyzed web server logs to identify malicious HTTP requests  
- Traced attacker’s IP, user-agent, and exploitation path  
- Identified unauthorized file uploads and web shell activity  
- Mapped attacker behavior to MITRE (T1190, T1105, T1505)  
**Findings:**  
- Server compromised via vulnerable upload endpoint  
- Attacker deployed a PHP web shell and modified site content  
**Tools:** Splunk, Wireshark, VirusTotal  
**Lessons Learned:**  
- Importance of WAF rules, file upload validation, and server hardening  

---

## 6. **IR_Ozarks**  
**Scenario:** Suspicious PowerShell execution on a workstation.  
**What I did:**  
- Investigated Sysmon logs for encoded PowerShell commands  
- Decoded Base64 payloads to reveal malicious downloader  
- Identified C2 communication attempts  
- Built detection logic for similar PowerShell misuse  
**Findings:**  
- User executed a malicious script leading to persistence attempts  
**Tools:** Sysmon, CyberChef, Splunk  
**Lessons Learned:**  
- PowerShell logging & Sysmon configuration are critical for IR  

---

## 5. **MiddleMayhem**  
**Scenario:** Multi-stage phishing → credential theft → lateral movement.  
**What I did:**  
- Reconstructed phishing email delivery path  
- Analyzed authentication logs for brute-force & password spraying  
- Identified lateral movement via SMB and RDP  
- Built timeline of attacker actions  
**Findings:**  
- Compromised user account used for internal reconnaissance  
**Tools:** ELK, Sysmon, Zeek  
**Lessons Learned:**  
- MFA enforcement and AD audit policies significantly reduce impact  

---

## 4. **Ozarks**  
**Scenario:** Suspicious outbound traffic from internal host.  
**What I did:**  
- Analyzed PCAP for beaconing patterns  
- Identified DNS tunneling behavior  
- Extracted payloads and enriched IOCs  
**Findings:**  
- Host infected with malware performing C2 over DNS  
**Tools:** Wireshark, Zeek, VirusTotal  
**Lessons Learned:**  
- DNS logging & anomaly detection are essential for SOC visibility  

---

## 3. **Piggy**  
**Scenario:** Unauthorized privilege escalation attempt.  
**What I did:**  
- Investigated Windows Event Logs for privilege abuse  
- Identified exploitation of vulnerable service configuration  
- Mapped attacker actions to MITRE (T1068, T1059)  
**Findings:**  
- Local privilege escalation succeeded due to misconfigured permissions  
**Tools:** Sysmon, Event Viewer, Splunk  
**Lessons Learned:**  
- Least privilege & service hardening prevent escalation attacks  

---

## 2. **SOC Alpha I / II**  
**Scenario:** Multi-part SOC analyst challenge simulating real MSSP workflow.  
**What I did:**  
- Performed alert triage on multiple log sources  
- Correlated events across endpoints, network, and authentication logs  
- Built detection rules for repeated malicious patterns  
**Findings:**  
- Identified coordinated attack chain involving phishing + malware  
**Tools:** Splunk, ELK, Sysmon  
**Lessons Learned:**  
- Importance of correlation searches and alert tuning in MSSP environments  

---

## 1. **The Walking Packets (Enterprise)**  
**Scenario:** Large-scale network compromise with lateral movement.  
**What I did:**  
- Investigated PCAP for scanning, SMB exploitation, and data exfiltration  
- Reconstructed attacker path across multiple hosts  
- Extracted malware artifacts and enriched IOCs  
**Findings:**  
- Attacker used automated worm-like propagation  
**Tools:** Wireshark, Zeek, CyberChef  
**Lessons Learned:**  
- Network segmentation and EDR visibility are critical for containment  

---

# Contact
If you are a recruiter or hiring manager, feel free to reach out.  
This portfolio demonstrates my hands-on SOC investigation capability and readiness for MSSP SOC Analyst roles.
