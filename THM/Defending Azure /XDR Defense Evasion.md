---
layout: default
title: Microsoft Defender XDR - Defense Evasion
---

# Microsoft Defender XDR: Defense Evasion

How attackers avoid getting caught, and how to spot and block it in Defender XDR.

## Common techniques

- **Obfuscation:** encoding or encrypting scripts so security tools can't read them
- **Impair defenses:** turning off AV, EDR or logging
- **Masquerading:** making malware look legitimate, for example naming it `svchost.exe` or using a double extension like `payslip.pdf.exe`
- **DLL side-loading:** getting a legitimate program to load a malicious DLL
- **Timestomping:** changing file timestamps so files don't stand out in a timeline
- **Signed binary proxy execution:** using trusted Windows binaries to run malicious code

## Impair defenses

This one stands out to me because it's the same idea as an attacker turning off CloudTrail in AWS. If they can switch off the thing watching them, nothing else gets logged. The fixes are similar too: tight permissions on who can change security settings, alerts when protection is turned off, and in Defender's case **tamper protection**, which stops Defender being disabled even by a local admin.

## Lab: attempt to turn off Defender

I investigated the incident **Attempt to turn off Microsoft Defender Antivirus protection**. The alert timeline showed `reg.exe`, started from `cmd.exe`, changing a registry value to disable Defender. The alert shows the old and new registry values side by side and the full command line, which can be copied for further investigation.

If it was confirmed as malicious, the next steps would be to collect an investigation package, start an automated investigation, open a live response session to check the registry, and isolate the device so the attacker can't use it to reach anything else.

## Detection

A simple hunting query for masquerading: `svchost.exe` running from anywhere other than System32.

```kql
DeviceProcessEvents
| where FileName =~ "svchost.exe" and FolderPath !startswith "C:\\Windows\\System32"
```

The real `svchost.exe` only ever runs from System32, so any result here is worth looking at.

## Prevention

- Turn on tamper protection (**Settings > Endpoints > Advanced features**)
- ASR rules in block mode: block Office apps injecting into other processes, block obfuscated scripts, block untrusted processes from USB, block Office apps creating child processes, and block executable content from email
- Turn on live response so analysts can work directly on an affected device

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### What is Defense Evasion

**When a threat actor tries to disable security tools, what is it referred to?**  
<span class="spoiler" tabindex="0">Impair Defense</span>

**When threat actors modify the timestamps of files and directories to avoid detection, what is it referred to?**  
<span class="spoiler" tabindex="0">Timestomping</span>

### Technique: Impair Defenses

**What should be enabled to prevent attackers from disabling antivirus and security controls?**  
<span class="spoiler" tabindex="0">tamper protection</span>

**Regular system updates and security solutions can mitigate known vulnerabilities that could be exploited to impair defenses. (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

### Technique: Masquerading

**What should be implemented to detect unauthorized changes to system or application files?**  
<span class="spoiler" tabindex="0">file integrity monitoring</span>

**In a Masquerading technique, do attackers disguise malicious files or processes with legitimate names to appear trustworthy? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

### Lab: Detect and Investigate

**Which command line tool initiated the Attempt to turn off Microsoft Defender Antivirus protection incident?**  
<span class="spoiler" tabindex="0">cmd.exe</span>

**What is the name of the user account involved?**  
<span class="spoiler" tabindex="0">THOMASPC\User</span>

**What is the value in the Malware detected field?**  
<span class="spoiler" tabindex="0">none</span>

### XDR: Prevent, Detect and Mitigate Defense Evasion Attacks

**Which advanced features allow security teams to have direct access to compromised devices to run commands, collect forensics, or manually remediate evasion techniques?**  
<span class="spoiler" tabindex="0">Live Response</span>

**What do security teams need to enable to prevent the impair defense technique?**  
<span class="spoiler" tabindex="0">Tamper Protection</span>
