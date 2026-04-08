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

## **-4/7/2026-PEAK2**  
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

## **-4/6/2026-PEAK**  
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

## 1. **Defaced (Enterprise)**  
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

## 2. **IR_Ozarks**  
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

## 3. **MiddleMayhem**  
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

## 5. **Piggy**  
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

## 6. **SOC Alpha I / II**  
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

## 7. **The Walking Packets (Enterprise)**  
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
