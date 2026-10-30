---
layout: default
title: Microsoft Defender XDR - Privilege Escalation
---

# Microsoft Defender XDR: Privilege Escalation

Getting from a normal user to admin, and investigating a UAC bypass in Defender XDR.

## Vertical and horizontal

- **Vertical:** going up a level, for example from a standard user to admin or root
- **Horizontal:** getting into another account at the same level, which might have access to different things

Common routes are software vulnerabilities and badly set permissions, for example a service running as SYSTEM whose file a normal user can overwrite.

## UAC bypass

UAC is the Windows "do you want to allow this app to make changes" prompt. A UAC bypass gets admin-level rights without the prompt appearing, usually by abusing a trusted Windows program that already runs elevated. A lot of them work by changing a registry key that the trusted program reads, so it ends up launching the attacker's command instead. The [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1548.002/T1548.002.md) repo has a list of known methods for testing.

Keeping Windows patched and not giving everyday users local admin rights removes most of the risk.

## Lab: UAC bypass alert

I opened **Multi-stage incident involving Privilege escalation on one endpoint** (incident 48) and went into the **UAC bypass was detected** alert.

- The MITRE ATT&CK techniques on the alert were Modify Registry and Bypass User Account Control, which matches how these bypasses usually work
- **Evidence** showed which files and processes were marked as suspicious
- The **process tree** confirmed the detection was accurate, by showing which process started which

Checking the process tree is the step I'd always do before escalating. It shows whether the chain of processes actually makes sense as an attack, or whether it's something like an admin tool behaving normally.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### What is Privilege Escalation

**What could be one of the starting points for privilege escalation?**  
<span class="spoiler" tabindex="0">misconfigured user account</span>

**What can privilege escalation lead to?**  
<span class="spoiler" tabindex="0">system compromise</span>

### Technique - Bypass User Account Control

**What kind of permissions does UAC bypass exploit in trusted applications?**  
<span class="spoiler" tabindex="0">elevated privileges</span>

**What can attackers install after bypassing UAC?**  
<span class="spoiler" tabindex="0">malware</span>

### Demo - Detect and Investigate

**What is the MITRE ATT&CK sub-technique ID used for this attack?**  
<span class="spoiler" tabindex="0">T1548.002</span>

**What is the PID for the suspicious process in this alert?**  
<span class="spoiler" tabindex="0">5628</span>
