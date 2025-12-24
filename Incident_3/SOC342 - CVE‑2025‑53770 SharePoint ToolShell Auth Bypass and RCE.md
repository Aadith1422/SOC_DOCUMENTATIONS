#  Incident 3 – SharePoint ToolShell Auth Bypass & Remote Code Execution

**Platform:** LetsDefend.io  
**Incident ID:** 320  
**Rule Name:** SOC342 – CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE  
**Severity:** Critical  
**Type:** Web Attack / Remote Code Execution  
**Status:** Closed – True Positive  

---

## 1. Incident Overview

A **critical SOC342 alert** was triggered after detection of suspicious unauthenticated POST requests targeting a vulnerable SharePoint endpoint. Investigation confirmed exploitation of the **ToolShell zero-day vulnerability (CVE-2025-53770)** on **SharePoint01**, resulting in **remote code execution**, **web shell deployment**, and **post-exploitation PowerShell activity**.

The attacker leveraged an authentication bypass to execute encoded PowerShell commands under the SharePoint IIS worker process, compiled a malicious payload on disk, and attempted to access sensitive ASP.NET configuration values, indicating **full server compromise**.

![Alert Overview](Screenshots/01_alert_overview.png)

---

## 2. Alert Metadata

- **Event Time:** Jul 22, 2025 – 01:07 PM  
- **Log Source:** Endpoint Security  
- **Affected Host:** SharePoint01  
- **Host IP:** 172.16.20.17  
- **Operating System:** Windows Server 2019  
- **HTTP Method:** POST  
- **Targeted URL:** `/_layouts/15/ToolPane.aspx?DisplayMode=Edit`  
- **User-Agent:** Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:120.0) Gecko/20100101 Firefox/120.0  
- **Device Action:** Allowed  

---

## 3. Initial Access – Web Exploitation

The attacker initiated unauthenticated POST requests to a known vulnerable SharePoint endpoint associated with ToolShell exploitation.

Key indicators of exploitation:
- Unauthenticated POST request
- Large request payload
- Abuse of SharePoint ToolPane functionality
- Spoofed referrer header

![Web Exploit Evidence](Screenshots/02_web_exploit_details.png)

---

## 4. Threat Intelligence Correlation

### Source IP Reputation
- **IP Address:** 107.191.58.76  
- **ASN:** AS20473 (Vultr)  
- **Country:** United States  
- **VirusTotal Detection Ratio:** 11 / 95  
- **Community Abuse Reports:** 26 independent reports  

Observed abuse categories:
- SharePoint RCE exploitation
- Web application attacks
- Brute force
- Port scanning
- Unauthorized access attempts

![Threat Intel IP](Screenshots/03_threat_intel_ip.png)

---

## 5. Exploitation & Execution Analysis

Following successful exploitation, the SharePoint IIS worker process (`w3wp.exe`) spawned encoded PowerShell commands.

```powershell
powershell.exe -nop -w hidden -EncodedCommand PCVAI...
```

Characteristics:
- NoProfile execution
- Hidden window
- Base64-encoded payload
- Execution from IIS context

![Encoded PowerShell](Screenshots/04_encoded_powershell.png)

---

## 6. Process Analysis

### Observed Process Chain
- `services.exe`  
  → `w3wp.exe` (SharePoint IIS Worker)  
  → `powershell.exe`  
  → `csc.exe`  

Notable behaviors:
- PowerShell execution from IIS
- Abuse of trusted Windows binaries
- Execution without user interaction

![Process Tree](Screenshots/05_process_tree.png)

---

## 7. Payload Compilation & Post-Exploitation

The attacker compiled a malicious .NET payload directly on the server.

```text
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe
/out:C:\Windows\Temp\payload.exe
```

This confirms a transition from fileless execution to disk-based malware.

---

## 8. Persistence & Configuration Abuse

PowerShell commands referenced sensitive ASP.NET configuration components.

### Indicators
- `MachineKeySection`
- `GetApplicationConfig`

Significance:
- Attempted machineKey extraction
- Enables forged authentication cookies
- Potential long-term persistence

![Terminal History](Screenshots/06_terminal_history.png)

---

## 9. Indicators of Compromise (IOCs)

| Type | Indicator | Description |
|---|---|---|
| IP | 107.191.58.76 | Attacker source IP |
| URL | /_layouts/15/ToolPane.aspx?DisplayMode=Edit | ToolShell exploit endpoint |
| File | spinstall0.aspx | Malicious ASPX web shell |
| File | payload.exe | Compiled malicious payload |
| String | MachineKeySection | Configuration access indicator |

---

## 10. MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059.001 – PowerShell |
| Defense Evasion | T1027 – Obfuscated / Encoded Command |
| Execution | T1127.001 – Trusted Developer Utilities (CSC.exe) |
| Persistence | T1505.003 – Web Shell |
| Credential Access | T1552.004 – Credentials from Configuration |
| Command & Control | T1105 – Ingress Tool Transfer |

---

## 11. Response & Containment

- SharePoint01 isolated to quarantine VLAN
- Attacker IP blocked at firewall and WAF
- Web shell identified and removed after forensic capture
- Malicious binaries eradicated

---

## 12. Recommended Actions

### Containment
- Maintain isolation of SharePoint01
- Block attacker infrastructure across perimeter devices

### Threat Hunting
- Search all SharePoint servers for:
  - `spinstall*.aspx`
  - `payload.exe` (filename/hash)
  - `MachineKeySection` and `GetApplicationConfig` usage

### Eradication
- Remove malicious files and unauthorized services
- Preserve forensic artifacts before deletion

### Mitigation
- Rotate ASP.NET machineKey values (coordinate with app owners)
- Apply Microsoft **July 2025 emergency SharePoint patches**
- Validate patch effectiveness

### Detect & Prevent
- Alert on encoded PowerShell (`-EncodedCommand`, `-e`) on web servers
- Alert on `csc.exe` compiling code in `C:\Windows\Temp`
- Enforce WAF rules blocking unauthenticated POSTs to ToolPane.aspx

---

## 13. Final Analyst Verdict

- **True Positive**
- Confirmed exploitation of **CVE-2025-53770**
- Remote code execution achieved
- Web shell deployed
- Credential material targeted
- Incident fully investigated, contained, and closed

---

## 14. Analyst Notes

This incident represents a real-world zero-day exploitation scenario involving authentication bypass, remote code execution, web shell persistence, and credential targeting. It highlights the necessity of rapid patching, layered defenses, and behavioral detections on internet-facing servers.

---