#  Incident 6 – Impersonating Domain MX Record Change Detected

**Platform:** LetsDefend.io  
**Incident ID:** 304  
**Rule Name:** SOC326 – Impersonating Domain MX Record Change Detected  
**Severity:** Medium  
**Type:** Threat Intelligence  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **SOC326 alert** was triggered when LetsDefend detected a suspicious **MX record change** for a domain closely resembling a legitimate organization domain. Attackers often modify MX records to reroute email traffic, enabling **phishing, credential harvesting, and business email compromise (BEC)**.

The investigation confirmed that the domain change was malicious in intent and aligned with known **email impersonation tactics**.

### 📸 Screenshot – Alert Overview  
![Alert Overview – SOC326 Trigger](Screenshots/01_alert_overview.png)



---

## 2. Alert Metadata

- **Event ID:** 304  
- **Event Time:** Sep 17, 2024 – 12:05 PM  
- **Date Closed:** Dec 24, 2025 – 09:01 AM  
- **Rule Level:** Security Analyst  
- **Source Address:** no-reply@cti-report.io  
- **Destination Address:** soc@letsdefend.io  
- **Device Action:** Allowed  


---

## 3. Domain & MX Record Analysis

### Domain Observed
- **Domain:** letsdefwnd[.]io  
- **Impersonated Target:** letsdefend.io  

### MX Record Change
- **Configured MX:** mail.mailerhost[.]net  

### Risk Analysis
- Domain name visually similar to legitimate brand  
- MX record points to external mail infrastructure  
- Common setup for phishing and spoofed email campaigns  

### 📸 Screenshot – Domain MX Record Change  

![Domain MX Record Change – letsdefwnd.io](Screenshots/03_mx_record_details.png)


---

## 4. Email Activity Analysis

### Email Details
- **From:** voucher@letsdefwnd.io  
- **To:** mateo@letsdefend.io  
- **Subject:** Congratulations! You've Won a Voucher  
- **Date:** Sep 18, 2024 – 08:00 AM  

### Observations
- Social engineering lure (reward-based phishing)  
- Brand impersonation using similar domain  
- Encourages user interaction via embedded button/link  

### 📸 Screenshot – Email Content  
![Phishing Email – Voucher Lure](Screenshots/04_email_content.png)

---

## 5. Log Management & Traffic Analysis

### Mail Flow Evidence
- Email successfully routed through modified MX  
- Exchange and firewall logs confirm delivery path  

### Raw Log Highlights
```
sender mail: voucher@letsdefwnd.io
recipient mail: mateo@letsdefend.io
```

### 📸 Screenshot – Raw Email Logs  
![Raw Email Logs – Mail Flow Evidence](Screenshots/05_raw_logs.png)


---

## 6. Threat Intelligence Correlation

Multiple IP addresses associated with the sending infrastructure were analyzed using VirusTotal.

### IP Reputation Summary

| IP Address | VT Detection |
|----------|-------------|
| 96.126.123.244 | 2/95 – Malicious |
| 45.56.79.23 | 6/95 – Malicious |
| 45.79.19.196 | 3/95 – Malicious |
| 173.255.194.134 | 1/95 – Malicious |
| 72.14.185.43 | 1/95 – Malicious |
| 45.33.30.197 | 2/95 – Malicious |

These IPs are hosted under **Akamai Connected Cloud**, a common abuse platform for transient phishing infrastructure.

### 📸 Screenshot – VirusTotal IP Analysis  
![VirusTotal – 96.126.123.244](Screenshots/06_vt_ip_1.png)
![VirusTotal – 45.56.79.23](Screenshots/07_vt_ip_2.png)
![VirusTotal – 45.79.19.196](Screenshots/08_vt_ip_3.png)
![VirusTotal – 173.255.194.134](Screenshots/09_vt_ip_4.png)
![VirusTotal – 72.14.185.43](Screenshots/10_vt_ip_5.png)
![VirusTotal – 45.33.30.197](Screenshots/11_vt_ip_6.png)


---

## 7. Indicators of Compromise (IOCs)

### Domains
- letsdefwnd[.]io  

### Email Addresses
- voucher@letsdefwnd.io  

### IP Addresses
- 96.126.123.244  
- 45.56.79.23  
- 45.79.19.196  
- 173.255.194.134  
- 72.14.185.43  
- 45.33.30.197  

---

## 8. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|-----------|
| Initial Access | T1566.002 – Phishing: Link |
| Reconnaissance | T1598 – Phishing for Information |
| Resource Development | T1583.001 – Acquire Infrastructure: Domains |
| Defense Evasion | T1036 – Masquerading |

---

## 9. Response & Actions Taken

- Domain impersonation confirmed  
- MX record abuse validated  
- Threat intelligence enrichment completed  
- No endpoint compromise observed  
- Case closed as **True Positive**  

---

## 10. Final Analyst Verdict

- **True Positive**  
- Successful detection of impersonation attempt  
- No user compromise recorded  
- Preventive detection worked as intended  

### 📸 Screenshot – Case Closure  
![Case Closure – True Positive](Screenshots/12_case_closure.png)


---

## 11. Analyst Notes

This incident demonstrates how **DNS and MX record manipulation** is leveraged to enable phishing campaigns. Continuous monitoring of domain changes and proactive threat intelligence correlation are critical to stopping impersonation attacks before user impact occurs.