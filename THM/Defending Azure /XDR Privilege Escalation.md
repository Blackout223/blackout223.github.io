---
layout: default
title: Microsoft Defender XDR - Privilege Escalation
---

# Microsoft Defender XDR: Privilege Escalation

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## What is Privilege Escalation

- **Vertical:** going up a level, for example from a standard user to admin or root
- **Horizontal:** getting into another account at the same level, which might have access to different things

Common routes are software vulnerabilities and badly set permissions, for example a service running as SYSTEM whose file a normal user can overwrite.

**What could be one of the starting points for privilege escalation?**  
<span class="spoiler" tabindex="0">misconfigured user account</span>


**What can privilege escalation lead to?**  
<span class="spoiler" tabindex="0">system compromise</span>


## Technique - Bypass User Account Control

UAC is the Windows "do you want to allow this app to make changes" prompt. A UAC bypass gets admin-level rights without the prompt appearing, usually by abusing a trusted Windows program that already runs elevated. A lot of them work by changing a registry key that the trusted program reads, so it ends up launching the attacker's command instead. The [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1548.002/T1548.002.md) repo has a list of known methods for testing.

Keeping Windows patched and not giving everyday users local admin rights removes most of the risk.

**What kind of permissions does UAC bypass exploit in trusted applications?**  
<span class="spoiler" tabindex="0">elevated privileges</span>


**What can attackers install after bypassing UAC?**  
<span class="spoiler" tabindex="0">malware</span>


## Demo - Detect and Investigate

I set the incident queue to 6 months and opened **Multi-stage incident involving Privilege escalation on one endpoint** (incident 48).

![Incidents](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741370246336.png)

![Time range set to 6 months](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1741370478468.png)

![Incident 48](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743600122516.png)

The **Alerts** tab lists each alert with its category. I opened **UAC bypass was detected**.

![Alerts tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743696239551.png)

![UAC bypass alert](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1743697164431.png)

The MITRE ATT&CK techniques on the alert were Modify Registry and Bypass User Account Control, which matches how these bypasses usually work.

![MITRE techniques in the room](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1744642923316.png)

![MITRE techniques on the alert](/THM/images/xdr/privesc-mitre.png)

**What is the MITRE ATT&CK sub-technique ID used for this attack?**  
<span class="spoiler" tabindex="0">T1548.002</span>


**Evidence** showed which entities were marked as suspicious.

![Evidence in the room](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1744643937150.png)

![Evidence on the alert](/THM/images/xdr/privesc-evidence.png)

**What is the PID for the suspicious process in this alert?**  
<span class="spoiler" tabindex="0">5628</span>


Finally, the **process tree** confirmed the detection was accurate by showing which process started which.

![Process tree](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/5efbaebdaaea011c857b438d-1744644166121.png)

Checking the process tree is the step I'd always do before escalating. It shows whether the chain of processes actually makes sense as an attack, or whether it's something like an admin tool behaving normally.
