#  Incident 8 – SQL Injection Detected (SOC127)

**Platform:** LetsDefend.io  
**Incident ID:** 235  
**Rule Name:** SOC127 – SQL Injection Detected  
**Severity:** High  
**Type:** Web Attack  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **SOC127 alert** was triggered after LetsDefend detected a **SQL Injection (SQLi) attack** targeting an internal web application hosted on **WebServer1000**.  
The malicious request contained **classic UNION-based SQL injection**, **cross-site scripting (XSS)**, and **command execution (`xp_cmdshell`)** payloads, indicating an advanced and deliberate exploitation attempt.

Analysis confirmed that the payload was successfully processed by the backend application, making this incident a **confirmed compromise attempt** requiring escalation.

---

## 2. Alert Metadata

- **Event ID:** 235  
- **Event Time:** Mar 07, 2024 – 12:51 PM  
- **Date Closed:** Dec 24, 2025 – 10:34 AM  
- **Rule Level:** Security Analyst  
- **Source IP Address:** 118.194.247.28  
- **Destination IP Address:** 172.16.20.12  
- **Destination Hostname:** WebServer1000  
- **Request Method:** GET  
- **Device Action:** Allowed  

### 📸 Screenshot – Alert Details  
![Alert Details](Screenshots/02_alert_details.png)

---

## 3. Payload & Exploitation Analysis

### Observed Malicious Request (URL Encoded)

The request contained a heavily obfuscated payload attempting:
- Authentication bypass
- Database enumeration
- Stored XSS injection
- OS command execution via database

### Decoded Payload
```http
GET /?douj=3034 AND 1=1 UNION ALL SELECT 1,NULL,
'<script>alert("XSS")</script>',table_name
FROM information_schema.tables
WHERE 2>1--/**/;
EXEC xp_cmdshell('cat ../../../../etc/passwd')
```

### Key Observations
- UNION-based SQL Injection
- Stored XSS payload embedded
- `xp_cmdshell` execution attempt
- Linux file system access (`/etc/passwd`)
- Strong indicator of successful SQLi execution

### 📸 Screenshot – URL Decode & Payload Analysis  
![Payload Decode](Screenshots/03_payload_decode.png)

---

## 4. Log Management & Traffic Analysis

### Log Findings
- Multiple requests observed from the same source IP
- sqlmap user-agent detected
- Destination port: **80**
- Automated exploitation behavior confirmed

### Traffic Direction
- **Inbound**
- External attacker targeting internal web server

### 📸 Screenshot – Raw Log Evidence  
![Raw Logs](Screenshots/04_raw_logs.png)

---

## 5. Threat Intelligence Correlation

### Source IP Reputation
- **IP Address:** 118.194.247.28  
- **ASN:** AS4808 – China Unicom Beijing Province Network  
- **VirusTotal Detection:** 7 / 95 vendors flagged as malicious  

### Known Behaviors
- SQL Injection
- Brute-force attempts
- Web exploitation scanning

### 📸 Screenshot – VirusTotal IP Reputation  
![VT IP](Screenshots/05_vt_ip.png)

---

## 6. Attack Impact Assessment

### Impact Summary
- SQL Injection payload successfully processed
- Potential database exposure
- Risk of data exfiltration
- Possible web shell or persistence setup
- High likelihood of further exploitation if not contained

### Risk Level
**High – Confirmed exploitation attempt**

---

## 7. Indicators of Compromise (IOCs)

### IP Addresses
- 118.194.247.28  

### URLs
- `/index.php?id=...` (SQLi payload)

### Techniques
- UNION-based SQL Injection
- XSS Injection
- xp_cmdshell execution

---

## 8. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|-----------|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059 – Command and Scripting Interpreter |
| Credential Access | T1555 – Credentials from Web Applications |
| Defense Evasion | T1027 – Obfuscated Files or Information |

---

## 9. Response & Actions Taken

- Incident validated as **True Positive**
- Attack confirmed as successful
- Escalated to **Tier 2**
- WebServer1000 recommended for isolation
- Artifacts added for correlation
- WAF tuning and URL blocklisting recommended

---

## 10. Final Analyst Verdict

- **True Positive**
- Successful SQL Injection attempt
- Immediate remediation required
- SOC controls detected but did not prevent execution

### 📸 Screenshot – Case Closure & Analyst Notes  
![Case Closure](Screenshots/06_case_closure.png)

---

## 11. Remediation Recommendations

- Enable and tune **WAF SQLi/XSS rules**
- Block malicious source IPs
- Disable risky database functions (`xp_cmdshell`)
- Apply least-privilege DB permissions
- Patch and harden the web application
- Conduct full forensic review of WebServer1000

---

## 12. Analyst Notes

This incident demonstrates how **poor input validation and exposed database functions** can lead to full application compromise.  
Continuous monitoring, WAF enforcement, and secure coding practices are essential to prevent similar SQL Injection attacks.