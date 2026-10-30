---
layout: default
title: Microsoft Defender XDR - Credential Access
---

[← Back to Defending Azure](/THM/defending-azure)

# Microsoft Defender XDR: Credential Access

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## What is Credential Access

Stealing usernames, passwords, hashes, tokens and keys, through brute force, keylogging, dumping credentials from memory, or abusing badly configured authentication. Once an attacker has real credentials they look like a normal user, which makes everything after this much harder to spot.

![What credential access leads to](/THM/images/xdr/credaccess-diagram.png)

**After a successful credential access tactic, can attackers blend in with legitimate users? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


**What is the process of extracting credentials from system memory called?**  
<span class="spoiler" tabindex="0">Credential Dumping</span>


**Credential access can lead to privilege escalation, lateral movement, and?**  
<span class="spoiler" tabindex="0">Persistence</span>


## Technique - Brute Force

Guessing passwords over and over until one works, or trying one common password against lots of accounts (password spraying). Defences: long and complex passwords, MFA, account lockout policies and CAPTCHAs.

**What factor increases the time required to crack a password exponentially?**  
<span class="spoiler" tabindex="0">password complexity</span>


**What should be implemented to mitigate automated multiple login attempts?**  
<span class="spoiler" tabindex="0">account lockout policies</span>


## Technique - Credential Dumping

Pulling credentials out of where Windows stores them:

- **LSASS memory:** the process that holds credentials for logged-in users, dumped with tools like Mimikatz or ProcDump
- **NTDS.dit:** the Active Directory database on domain controllers, which has every domain user's password hash
- **LSA secrets:** service account passwords
- **DCSync:** pretending to be a domain controller to ask a real one for password hashes

Defences: Credential Guard to protect LSASS, least privilege so fewer accounts can reach these places, turning off NTLM where possible, and MFA so stolen passwords alone aren't enough.

**As security admins, what control can be implemented to prevent the use of dumped credentials?**  
<span class="spoiler" tabindex="0">Multi-Factor Authentication</span>


**What database do attackers target during NTDS dumping?**  
<span class="spoiler" tabindex="0">NTDS.dit</span>


## Lab - Detect and Investigate Using Defender XDR

From the device menu on the alert page, **Go hunt** jumps straight into advanced hunting for that device.

![Go hunt in the device menu](/THM/images/xdr/credaccess-gohunt.png)

**What do you click to go to the advanced hunting page of a specific device from the alert page?**  
<span class="spoiler" tabindex="0">Go hunt</span>


The alert was **Password spraying**, and it showed a PowerShell script had been downloaded and run that tries one password against every account in the domain.

![Password spraying alert](/THM/images/xdr/credaccess-spray.png)

**What is the name of the PowerShell script that was executed?**  
<span class="spoiler" tabindex="0">DomainPasswordSpray.ps1</span>


## XDR: Prevent, Detect and Mitigate Credential Access

**Detection**

Incident email notifications, plus the **Credential Access** community queries in advanced hunting.

![Incident email notification rule](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744882501503.png)

![Credential access community queries](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1749670681223.png)

These are the queries from the room I'd keep for my own use.

LSASS being accessed by common dumping tools:

```kql
DeviceProcessEvents
| where FileName in~ ("procdump.exe", "mimikatz.exe", "taskmgr.exe")
| where ProcessCommandLine contains "lsass"
```

The SAM, SECURITY or NTDS files being read:

```kql
DeviceFileEvents
| where FileName in~ ("SAM", "SECURITY", "SYSTEM", "ntds.dit")
| where FolderPath has_any ("\\Windows\\System32\\config", "C:\\Windows\\NTDS")
| where ActionType == "FileRead"
```

PowerShell with credential-dumping keywords:

```kql
DeviceProcessEvents
| where FileName =~ "powershell.exe"
| where ProcessCommandLine has_any ("Get-Credential", "Invoke-Mimikatz", "DumpCreds", "lsass")
| project Timestamp, DeviceName, ProcessCommandLine, InitiatingProcessAccountName
```

Matching on tool names like `mimikatz.exe` is easy to get around by renaming the file, so these are a starting point rather than full coverage. Looking at what's accessing LSASS, rather than what the file is called, is more reliable.

**Prevention**

- ASR rule: block executable content from email client and webmail, so the tools never arrive in the first place
- Safe Links in Defender for Office 365, to block sites hosting malicious scripts
- Credential Guard

**Which ASR rule will prevent the delivery of initial payloads that often include credential dumping tools?**  
<span class="spoiler" tabindex="0">Block executable content from email client and webmail</span>


**Which Defender for Office 365 policy will block access to websites hosting malicious scripts or executables?**  
<span class="spoiler" tabindex="0">Safe Links</span>


**What should be configured to protect the Local Security Authority Subsystem Service from attackers and stop credential dumping memory?**  
<span class="spoiler" tabindex="0">Credential Guard</span>

