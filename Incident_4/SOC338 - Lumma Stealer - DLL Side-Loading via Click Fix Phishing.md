# Incident 4 – Lumma Stealer via ClickFix Phishing (DLL Side-Loading)

**Platform:** LetsDefend.io  
**Incident ID:** 316  
**Rule Name:** SOC338 – Lumma Stealer – DLL Side-Loading via ClickFix Phishing  
**Severity:** Critical  
**Type:** Data Leakage  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **critical SOC338 alert** was triggered following the detection of a phishing email impersonating a legitimate **Windows 11 upgrade notification**. The email redirected the user to a malicious website hosting a **ClickFix-style phishing page**, which attempted to trick the victim into executing malicious content.

Investigation confirmed the phishing campaign was associated with **Lumma Stealer**, a widely used information-stealing malware. The attack relied on **social engineering and DLL side-loading** techniques to deliver the payload, posing a **high risk of credential theft and data exfiltration**.

### 📸 Screenshot – Alert Overview
![Alert Overview](Screenshots/01_alert_overview.png)


---

## 2. Alert Metadata

- **Event Time:** Mar 13, 2025 – 09:44 AM  
- **Date Closed:** Dec 24, 2025 – 07:39 AM  
- **Log Source:** Email Security  
- **Affected User:** Dylan  
- **Host Name:** Dylan  
- **Host IP:** 172.16.17.216  
- **Operating System:** Windows 10 (64-bit)  
- **Device Action:** Allowed  

### 📸 Screenshot – Alert Details
![Alert Details](Screenshots/02_alert_details.png)


---

## 3. Email Analysis

### Email Details
- **From:** update@windows-update.site  
- **To:** dylan@letsdefend.io  
- **Subject:** Upgrade your system to Windows 11 Pro for FREE  
- **SMTP Source IP:** 132.232.40.201  

### Observations
- Spoofed sender domain impersonating Microsoft
- Urgent call-to-action
- HTML content mimicking Windows branding
- Embedded malicious redirect URL

### 📸 Screenshot – Email Content
![Email Content](Screenshots/03_email_content.png)


---

## 4. Phishing Website & URL Analysis

### Malicious URL
https://windows-update.site/

### Findings
- Fake Microsoft upgrade page
- ClickFix-style interaction
- **10/98** VirusTotal detections

### 📸 Screenshot – Phishing Page
![Phishing Page](Screenshots/04_phishing_page.png)


---

## 5. Endpoint & User Activity Analysis

### Browser History
- User accessed the phishing domain from endpoint Dylan

### 📸 Screenshot – Browser History
![Browser History](Screenshots/05_browser_history.png)


---

## 6. Malware Technique Analysis

- **Malware Family:** Lumma Stealer
- **Delivery:** ClickFix Phishing
- **Execution Method:** DLL Side-Loading

### Capabilities
- Credential theft
- Browser data extraction
- Crypto wallet targeting
- Data exfiltration

---

## 7. Indicators of Compromise (IOCs)

### Network
- Domain: windows-update.site
- SMTP IP: 132.232.40.201

### Malware
- Lumma Stealer
- DLL Side-Loading

---

## 8. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|---------|
| Initial Access | T1566.002 – Phishing: Link |
| Execution | T1204.001 – User Execution |
| Defense Evasion | T1574.002 – DLL Side-Loading |
| Credential Access | T1555 – Credentials from Password Stores |
| Collection | T1005 – Data from Local System |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

---

## 9. Response & Actions Taken

- Phishing email validated
- URL reputation confirmed
- Endpoint reviewed (no execution)
- Incident closed as True Positive

---

## 10. Final Analyst Verdict

- **True Positive**
- Lumma Stealer delivery attempt blocked
- No persistence observed
- Incident fully resolved

---

## 11. Analyst Notes

This incident reinforces the effectiveness of phishing as an initial access vector and highlights the importance of email security controls, user awareness, and proactive threat hunting.