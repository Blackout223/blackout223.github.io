---
layout: default
title: Microsoft Defender XDR - Lateral Movement
---

[← Back to Defending Azure](/THM/defending-azure)

# Microsoft Defender XDR: Lateral Movement

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## What is Lateral Movement

Moving from the first compromised machine to others on the network, looking for more access or valuable data. Usually done with stolen credentials (dumped passwords, pass-the-hash) and remote tools like PsExec or RDP, or badly secured file shares. Network segmentation, least privilege, MFA and alerts on unusual logins all make it harder.

**What is one technique that attackers rely on to execute lateral movement?**  
<span class="spoiler" tabindex="0">credential dumping</span>


## WinLNK malware

Malicious Windows shortcut (`.lnk`) files. They look like normal shortcuts, but clicking one runs a hidden command. Attackers leave them on shared drives and USB sticks so they spread to whoever opens them, and often use them to drop something else like a RAT or a credential stealer. Watching shared folders for new `.lnk` files and blocking unknown shortcuts from running both help.

**What is the file format of WinLNK malware?**  
<span class="spoiler" tabindex="0">shortcut file</span>


**When a victim clicks on a compromised file, what can be triggered?**  
<span class="spoiler" tabindex="0">hidden command</span>


**What is one place malicious shortcut files can be found?**  
<span class="spoiler" tabindex="0">shared folders</span>


## Mimikatz credential theft tool

Mimikatz pulls passwords, hashes and Kerberos tickets out of LSASS memory. For lateral movement it's used for:

- **Pass-the-hash:** logging in with an NTLM hash, no password needed
- **Pass-the-ticket:** reusing a stolen Kerberos ticket
- **Overpass-the-hash:** turning an NTLM hash into a Kerberos ticket

Because these use real credentials, they don't look like brute forcing and don't set off lockouts. Defences: least privilege, turning off WDigest (which can store plaintext passwords in memory), LSA protection, Credential Guard, removing debug rights from admins, and not caching passwords on machines.

**What is Mimikatz designed to extract?**  
<span class="spoiler" tabindex="0">authentication credentials</span>


**What does Mimikatz directly interact with?**  
<span class="spoiler" tabindex="0">Local Security Authority Subsystem Service</span>


**What should organizations enforce to defend against Mimikatz and credential theft?**  
<span class="spoiler" tabindex="0">least privilege access</span>


## Lab: Detect and Investigate

I set the incident queue to 6 months and opened **Multi-stage incident involving Execution & Lateral movement on one endpoint** (incident 42).

![Incidents](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741370246336.png)

![Time range set to 6 months](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741370478468.png)

![Incident 42](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741370830120.png)

![Incident tabs](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741371248789.png)

![Incident tabs continued](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741371248827.png)

**Attack story** replays the alerts in order on a graph, so you can see how the attack spread.

![Attack story in the room](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1742573436725.png)

![Attack story, latest alert](/THM/images/xdr/lateral-attack-story.png)

**What is the latest alert in the attack story?**  
<span class="spoiler" tabindex="0">DLL search order hijack</span>


**Alerts** lists everything linked to the incident. The Mimikatz alert's timeline showed Defender had blocked it from running, and the alert was already resolved.

![Alerts tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1742574170670.png)

![Mimikatz alert](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1742574604536.png)

![Alert timeline](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743179851421.png)

![Mimikatz blocked](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743179851399.png)

![Alert resolved](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743179851400.png)

**Investigations** shows the automated investigations Defender ran.

![Investigations tab](/THM/images/xdr/lateral-investigations.png)

**What is the investigation status for the alert 'Winlnk' malware was detected?**  
<span class="spoiler" tabindex="0">Remediated</span>


**Evidence and Response** lists the files, processes, URLs and IPs involved.

![Evidence and Response, files](/THM/images/xdr/lateral-evidence.png)

**What was the suspicious entity?**  
<span class="spoiler" tabindex="0">test10.lnk</span>


**Assets** shows the devices and users involved, and the response actions available for each.

![Assets tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743197780543.png)

![Device response actions](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743199117684.png)

It's a good example of why XDR is useful: the shortcut file, the Mimikatz attempt and the DLL hijack were all in one incident, instead of being separate alerts someone has to link together by hand.
