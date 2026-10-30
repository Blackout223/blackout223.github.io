---
layout: default
title: Microsoft Defender XDR - Lateral Movement
---

# Microsoft Defender XDR: Lateral Movement

Moving from the first compromised machine to others on the network, looking for more access or valuable data.

## How it's done

Usually with stolen credentials: dumped passwords, pass-the-hash or pass-the-ticket. Then remote tools like PsExec or RDP, or badly secured file shares. Network segmentation, least privilege, MFA and alerts on unusual logins all make it harder.

## WinLNK malware

Malicious Windows shortcut (`.lnk`) files. They look like normal shortcuts, but clicking one runs a hidden command. Attackers leave them on shared drives and USB sticks so they spread to whoever opens them, and often use them to drop something else like a RAT or a credential stealer. Watching shared folders for new `.lnk` files and blocking unknown shortcuts from running both help.

## Mimikatz

Mimikatz pulls passwords, hashes and Kerberos tickets out of LSASS memory. For lateral movement it's used for:

- **Pass-the-hash:** logging in with an NTLM hash, no password needed
- **Pass-the-ticket:** reusing a stolen Kerberos ticket
- **Overpass-the-hash:** turning an NTLM hash into a Kerberos ticket

Because these use real credentials, they don't look like brute forcing and don't set off lockouts. Defences: least privilege, turning off WDigest (which can store plaintext passwords in memory), LSA protection, Credential Guard, removing debug rights from admins, and not caching passwords on machines.

## Lab: investigating the incident

I opened **Multi-stage incident involving Execution & Lateral movement on one endpoint** (incident 42) and went through each tab:

- **Attack story:** replays the alerts in order on a graph, so you can see how the attack spread. The latest alert was a DLL search order hijack.
- **Alerts:** the Mimikatz alert's timeline showed Defender had blocked it from running, and the alert was already resolved.
- **Assets:** the devices and users involved, and the response actions available for each one.

The WinLNK alert had been remediated, and the suspicious entity was a shortcut file. It's a good example of why XDR is useful: the shortcut, the Mimikatz attempt and the DLL hijack were all on one incident, instead of being separate alerts someone has to link together by hand.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### What is Lateral Movement

**What is one technique that attackers rely on to execute lateral movement?**  
<span class="spoiler" tabindex="0">credential dumping</span>

### WinLNK malware

**What is the file format of WinLNK malware?**  
<span class="spoiler" tabindex="0">shortcut file</span>

**When a victim clicks on a compromised file, what can be triggered?**  
<span class="spoiler" tabindex="0">hidden command</span>

**What is one place malicious shortcut files can be found?**  
<span class="spoiler" tabindex="0">shared folders</span>

### Mimikatz credential theft tool

**What is Mimikatz designed to extract?**  
<span class="spoiler" tabindex="0">authentication credentials</span>

**What does Mimikatz directly interact with?**  
<span class="spoiler" tabindex="0">Local Security Authority Subsystem Service</span>

**What should organizations enforce to defend against Mimikatz and credential theft?**  
<span class="spoiler" tabindex="0">least privilege access</span>

### Lab: Detect and Investigate

**What is the latest alert in the attack story?**  
<span class="spoiler" tabindex="0">DLL search order hijack</span>

**What is the investigation status for the alert 'Winlnk' malware was detected?**  
<span class="spoiler" tabindex="0">Remediated</span>

**What was the suspicious entity?**  
<span class="spoiler" tabindex="0">test10.lnk</span>
