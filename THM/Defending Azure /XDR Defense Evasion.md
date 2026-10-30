---
layout: default
title: Microsoft Defender XDR - Defense Evasion
---

[← Back to Defending Azure](/THM/defending-azure)

# Microsoft Defender XDR: Defense Evasion

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## What is Defense Evasion

How attackers avoid getting caught. Common techniques:

- **Obfuscation:** encoding or encrypting scripts so security tools can't read them
- **Impair defenses:** turning off AV, EDR or logging
- **Masquerading:** making malware look legitimate
- **DLL side-loading:** getting a legitimate program to load a malicious DLL
- **Timestomping:** changing file timestamps so files don't stand out in a timeline
- **Signed binary proxy execution:** using trusted Windows binaries to run malicious code

**When a threat actor tries to disable security tools, what is it referred to?**  
<span class="spoiler" tabindex="0">Impair Defense</span>


**When threat actors modify the timestamps of files and directories to avoid detection, what is it referred to?**  
<span class="spoiler" tabindex="0">Timestomping</span>


## Technique: Impair Defenses

Disabling security tools, turning off or tampering with logs, or changing security policies. It's the same idea as an attacker turning off CloudTrail in AWS: if they can switch off the thing watching them, nothing else gets logged.

The fixes are similar too: tight permissions on who can change security settings, alerts when protection is turned off, keeping things patched, and in Defender's case **tamper protection**, which stops Defender being disabled even by a local admin.

**What should be enabled to prevent attackers from disabling antivirus and security controls?**  
<span class="spoiler" tabindex="0">tamper protection</span>


**Regular system updates and security solutions can mitigate known vulnerabilities that could be exploited to impair defenses. (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


## Technique: Masquerading

Making malware look legitimate: double extensions like `payslip.pdf.exe`, naming files after Windows processes like `svchost.exe`, or signing malware with a fake certificate. Defences: user awareness training, EDR watching for processes running from odd places, only trusting signed code, and file integrity monitoring.

**What should be implemented to detect unauthorized changes to system or application files?**  
<span class="spoiler" tabindex="0">file integrity monitoring</span>


**In a Masquerading technique, do attackers disguise malicious files or processes with legitimate names to appear trustworthy? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


## Lab: Detect and Investigate

I searched the incident queue for **Attempt to turn off Microsoft Defender Antivirus protection**.

![Searching the incident queue](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1748075220014.png)

![The incident](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744743828239.png)

The incident shows the device, user, time and category (Defense Evasion).

![Incident details](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744743828113.png)

The alert timeline showed `reg.exe`, started from `cmd.exe`, changing a registry value to disable Defender. It shows the old and new registry values side by side and the full command line, which can be copied for further investigation.

![Alert timeline](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744646981609.png)

![Alert details](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744792738297.png)

![Initiating process](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744646982284.png)

![Alert actions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744792434699.png)

**Which command line tool initiated the Attempt to turn off Microsoft Defender Antivirus protection incident?**  
<span class="spoiler" tabindex="0">cmd.exe</span>


**What is the name of the user account involved?**  
<span class="spoiler" tabindex="0">THOMASPC\User</span>


**What is the value in the Malware detected field?**  
<span class="spoiler" tabindex="0">none</span>


If it was confirmed as malicious, the next steps would be to collect an investigation package, start an automated investigation, open a live response session to check the registry, and isolate the device so the attacker can't use it to reach anything else.

![Device response actions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744792738294.png)

## XDR: Prevent, Detect and Mitigate Defense Evasion Attacks

**Detection**

Incident email notifications, plus the **Defense evasion** community queries in advanced hunting.

![Incident email notification rule](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744794248067.png)

A simple hunting query for masquerading: `svchost.exe` running from anywhere other than System32. The real one only ever runs from there, so any result is worth looking at.

```kql
DeviceProcessEvents
| where FileName =~ "svchost.exe" and FolderPath !startswith "C:\\Windows\\System32"
```

![svchost hunting query](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744640140239.png)

**Prevention**

- ASR rules in block mode: block Office apps injecting into other processes, block obfuscated scripts, block untrusted processes from USB, block Office apps creating child processes, and block executable content from email
- Tamper protection on, under **Settings > Endpoints > Advanced features**

![Tamper protection](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744644942365.png)

**Response**

Turn on live response so analysts can work directly on an affected device.

![Live response](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744645154260.png)

**Which advanced features allow security teams to have direct access to compromised devices to run commands, collect forensics, or manually remediate evasion techniques?**  
<span class="spoiler" tabindex="0">Live Response</span>


**What do security teams need to enable to prevent the impair defense technique?**  
<span class="spoiler" tabindex="0">Tamper Protection</span>

