---
layout: default
title: Microsoft Defender XDR - Execution
---

[← Back to Defending Azure](/THM/defending-azure)

# Microsoft Defender XDR: Execution

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## Execution Attack Tactics

[Execution](https://attack.mitre.org/tactics/TA0002/) is the stage where an attacker gets their code running on a machine. Common techniques:

- **Command line and scripts:** PowerShell, cmd or bash
- **Malicious files:** .exe, .bat, .vbs, .js and so on
- **Scheduled tasks and cron jobs:** run something on a timer, which also gives persistence
- **Exploiting vulnerabilities** in software to run code

**What technique is it when a threat actor tries to use command-line tools to execute malicious commands?**  
<span class="spoiler" tabindex="0">command and scripting</span>


## Technique - PowerShell

PowerShell is the big one on Windows because it's already installed and trusted. Attackers use it to download and run scripts straight into memory, run base64-encoded commands, or switch off Defender with something like `Set-MpPreference -DisableRealtimeMonitoring $true`.

What to watch for:

- Script block logging (event 4104) and process creation (event 4688)
- `-EncodedCommand` in the command line
- PowerShell started by Word or Excel, which usually means a malicious macro

What helps prevent it: Constrained Language Mode, removing PowerShell v2 (it doesn't have the newer logging), and ASR rules.

**As part of mitigation, what rules can security admins configure to block suspicious script execution?**  
<span class="spoiler" tabindex="0">Attack Surface Reduction</span>


**Aside from running it locally, which other way can threat actors run PowerShell commands to execute a malicious payload?**  
<span class="spoiler" tabindex="0">remotely</span>


## Technique - Scheduled Task/Job

For example `schtasks /create /tn "UpdateTask" /tr "C:\malware.exe" /sc daily /st 12:00` on Windows, or a cron job on Linux. To detect it, watch for new tasks being created and for `schtasks.exe` running. To prevent it, limit who can create tasks, especially on remote machines.

**What can be implemented to prevent unauthorised administrators from setting up scheduled tasks on remote systems?**  
<span class="spoiler" tabindex="0">Least Privilege Principle</span>


## Lab - Detect and Investigate

I set the incident queue to 6 months and opened **Hands-on keyboard attack was launched from a compromised account (attack disruption)** (incident 49).

![Incident queue set to 6 months](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1743714809242.png)

![Incident 49](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1752849098381.png)

Inside it was the alert **A script with suspicious content was observed**.

![Script alert in the incident](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1752849098379.png)

![Alert page](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1752849098350.png)

The alert timeline showed powershell.exe running a script, and the script content can be copied out for further analysis.

![Alert timeline](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1743716558988.png)

![Remote execution details](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1752849098349.png)

![Alert severity](/THM/images/xdr/execution-severity.png)

**What is the severity of the alert "A script with suspicious content was observed"?**  
<span class="spoiler" tabindex="0">medium</span>


![Incident details](/THM/images/xdr/execution-incident.png)

**What is the integrity level of the alert "A script with suspicious content was observed"?**  
<span class="spoiler" tabindex="0">High</span>


![Alert category](/THM/images/xdr/execution-category.png)

**What is the category of the alert "A script with suspicious content was observed"?**  
<span class="spoiler" tabindex="0">Execution</span>


From the device menu, the response options are: run an AV scan, collect an investigation package, restrict app execution, start an automated investigation, open a live response session, isolate the device, or click **Go hunt** to pivot into advanced hunting for that device.

![Device response actions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1752849099894.png)

## XDR: Prevent, Detect and Mitigate Execution Attacks

**Detection**

- Make sure every device is onboarded to Defender for Endpoint and incident email notifications are set up

![Incident email notification rule](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744882501503.png)

- Community queries under **Advanced hunting > Queries > Community queries > Execution** are a good starting point

![Execution community queries](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744227099001.png)

**Prevention**

- ASR rules in block mode: block potentially obfuscated scripts, block executable content from email, and block JavaScript or VBScript from launching downloaded content
- Defender for Cloud Apps policies, under **Cloud apps > Policy management**

![Cloud apps policy management](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744232885290.png)

- Defender for Office 365: Safe Attachments, Safe Links, anti-phishing and zero-hour auto purge, under **Email & collaboration > Policies & rules > Threat policies**

![Email and collaboration policies](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744232885504.png)

**Response**

Automated investigation and response (AIR) and automatic attack disruption, which need the right licences, the Defender products deployed, and device groups set to full remediation.

**On Microsoft Defender for Office 365, what security control will prevent the execution of harmful scripts or malware embedded in attachments?**  
<span class="spoiler" tabindex="0">Safe Attachment</span>


**On Microsoft Defender for Endpoint, what ASR rule do you need to prevent scripts from launching potentially malicious downloaded content?**  
<span class="spoiler" tabindex="0">Block JavaScript or VBScript from launching downloaded executable content</span>


**The Microsoft Defender for Endpoint's device discovery should be set to 'standard discovery' is a prerequisite for which mitigation and response?**  
<span class="spoiler" tabindex="0">automatic attack disruption</span>

