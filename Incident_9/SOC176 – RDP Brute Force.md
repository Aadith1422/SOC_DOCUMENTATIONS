#  Incident 9 – RDP Brute Force Detected (Successful Compromise)

**Platform:** LetsDefend.io  
**Incident ID:** 234  
**Rule Name:** SOC176 – RDP Brute Force Detected  
**Severity:** Medium  
**Type:** Brute Force  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **SOC176 alert** was triggered after multiple failed **RDP authentication attempts** were detected from a single external source targeting an internal Windows host.  
Subsequent log analysis confirmed a **successful login**, escalating the incident from brute-force attempt to **confirmed system compromise**.

![Alert Overview](Screenshots/01_alert_overview.png)

---

## 2. Alert Metadata

- **Event ID:** 234  
- **Event Time:** Mar 07, 2024 – 11:44 AM  
- **Date Closed:** Dec 24, 2025 – 12:15 PM  
- **Rule Level:** Security Analyst  
- **Source IP:** 218.92.0.56  
- **Destination IP:** 172.16.17.148  
- **Hostname:** Matthew  
- **Protocol:** RDP (TCP/3389)  
- **Firewall Action:** Allowed  


---

## 3. Brute Force Activity Analysis

Log analysis revealed repeated failed authentication attempts using multiple usernames such as **admin**, **guest**, and **sysadmin**, consistent with a password spraying technique.

![Failed Login Attempts](Screenshots/03_failed_logins.png)

---

## 4. Successful Authentication Evidence

A successful RDP login was detected shortly after the failed attempts.

- **Windows Event ID:** 4624  
- **Username:** Matthew  
- **Source IP:** 218.92.0.56  

This confirms credential compromise.

![Successful Login](Screenshots/04_successful_login.png)

---

## 5. Log Management Correlation

Firewall and OS logs confirmed repeated inbound connections from the same source IP to port **3389**, validating brute-force behavior.

![Log Management View](Screenshots/05_log_management.png)

---

## 6. Threat Intelligence Correlation

The source IP was checked against VirusTotal and Threat Intel feeds.

- **ASN:** AS4134 (ChinaNet)  
- **VirusTotal:** 7/95 vendors flagged as malicious  
- **Associated with:** SSH/RDP brute force campaigns  

![VirusTotal IP Analysis](Screenshots/06_vt_ip.png)

---

## 7. Endpoint Network Activity

Endpoint telemetry shows outbound and inbound connections immediately following the successful login, indicating an active interactive session.

![Endpoint Network Activity](Screenshots/07_network_activity.png)

---

## 8. Process Execution Analysis (Post-Compromise Validation)

Endpoint process monitoring confirmed interactive session establishment.

- **Process:** winlogon.exe  
- **Parent:** smss.exe  
- **Execution Time:** Mar 07, 2024 – 11:44:54  
- **User Context:** NT AUTHORITY\SYSTEM  

This validates a real RDP session.

![Process Execution](Screenshots/08_process_execution.png)

---

## 9. Indicators of Compromise (IOCs)

### IP Addresses
- 218.92.0.56  

### Ports
- TCP/3389 (RDP)

---

## 10. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|-----------|
| Credential Access | T1110 – Brute Force |
| Initial Access | T1078 – Valid Accounts |
| Lateral Movement | T1021.001 – RDP |

---

## 11. Response & Actions Taken

- Incident confirmed as **True Positive**  
- Escalated to **Tier 2 SOC**  
- Endpoint isolation recommended  
- Forced password reset for affected account  
- RDP access hardening advised  

---

## 12. Final Analyst Verdict

- **True Positive**  
- Successful RDP compromise confirmed  
- Immediate containment required  

![Case Closure](Screenshots/09_case_closure.png)

---

## 13. Analyst Notes

This incident demonstrates how exposed RDP services remain a high-risk attack surface.  
Strong password policies, MFA, IP allowlisting, and continuous monitoring are critical to preventing similar compromises.