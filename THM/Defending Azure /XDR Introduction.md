---
layout: default
title: Microsoft Defender XDR - Introduction
---

[← Back to Defending Azure](/THM/defending-azure)

# Microsoft Defender XDR: Introduction

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## What is Microsoft Defender XDR

EDR only sees endpoints: laptops, desktops and servers. XDR pulls in signals from endpoints, email, identities and cloud apps, and links them together. So a phishing email, the malware it dropped and the account it then abused can show up as one incident instead of three unrelated alerts in three different tools.

![Microsoft Defender portal home](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744057080489.gif)

In the Defender portal that means one place for incidents and alerts, hunting, response actions and threat intelligence.

**What is EDR (Endpoint Detection and Response) visibility limited to?**  
<span class="spoiler" tabindex="0">Endpoints</span>


**Is Microsoft Defender XDR purpose-built for organizations using Microsoft products? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


## Microsoft Defender XDR Architecture

![Defender products sharing signals with Defender XDR](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744728619507.svg)

- **Defender for Endpoint:** EDR, antivirus and vulnerability management on devices
- **Defender for Office 365:** email, links and attachments
- **Defender for Identity:** on-prem Active Directory, catching things like lateral movement with stolen accounts
- **Defender for Cloud Apps:** cloud app usage, including apps IT hasn't approved
- **Entra ID Protection:** risky sign-ins, used together with Conditional Access

Defender XDR correlates all of it, can respond automatically (isolating devices, disabling accounts, revoking tokens), and lets you hunt across everything with KQL.

**Which of the Defender XDR signals focuses on email security?**  
<span class="spoiler" tabindex="0">Microsoft Defender for Office 365</span>


**Which querying language does Microsoft Defender XDR use for threat hunting?**  
<span class="spoiler" tabindex="0">Kusto Query Language</span>


## Microsoft Defender XDR Settings

The room walks through a typical attack and which product would catch each stage:

![An attack chain and the Defender product that stops each stage](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744800370913.svg)

1. A phishing email arrives. Exchange Online Protection and mail flow rules can stop it.
2. Safe Links and Safe Attachments check any links or files that get through.
3. Defender for Endpoint finds and fixes vulnerabilities before malware can use them.
4. Defender for Identity flags odd account changes and lateral movement.
5. Defender for Cloud Apps spots data being taken out, like impossible travel or unusual downloads.

It's basically defence in depth: if one layer misses it, the next one has a chance.

XDR-wide settings are under **Settings > Microsoft Defender XDR**. This includes email notifications, alert tuning, critical asset management, and excluding users from automated responses.

![Microsoft Defender XDR settings](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744112778767.png)

**Monitoring risky Lateral movement is a feature of which Microsoft Defender product?**  
<span class="spoiler" tabindex="0">Defender for Identity</span>


**Where can security admins exclude users from automated response actions?**  
<span class="spoiler" tabindex="0">identity automated response</span>


## Microsoft Defender XDR Roles and Permissions

Access can be given with Entra ID roles (like Security Administrator or Security Reader) or with custom roles for each Defender product, under **Permissions**.

![Permissions page for each Defender product](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744152357611.png)

![Example Defender for Endpoint role settings](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744152357831.png)

**From which tab can security admins create custom roles for specific Microsoft Defender Products?**  
<span class="spoiler" tabindex="0">Permissions</span>


## Microsoft Defender XDR Unified RBAC

Unified RBAC manages permissions for all the Defender products in one place, which is the cleanest way to do least privilege. It has to be switched on first, either from **Permissions > Roles** (Activate workloads or Workload settings) or from **Settings > Microsoft Defender XDR > Permissions and roles**.

![Activate workloads and Workload settings](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744193570086.png)

![Activate unified RBAC](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744193621906.png)

**How many ways can Microsoft Defender XDR unified role-based access control (RBAC) be activated?**  
<span class="spoiler" tabindex="0">2</span>


**Aside from the Activate workload option, which other option can be used to activate Microsoft Defender XDR unified role-based access control (RBAC) from the permissions and roles page?**  
<span class="spoiler" tabindex="0">workload settings</span>


## Lab: Navigating Defender XDR Portal

I went through the main areas of the portal.

![Defender navigation pane](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744205909989.png)

The incident queue, and **Multi-stage incident involving Privilege escalation on one endpoint** (incident 48):

![Incident queue](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744207510762.png)

![Incident page](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744208907192.png)

![Incidents and alerts tabs](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744214390143.png)

**Besides the Incidents and Email & collaboration alerts tabs, what other tab can be found under Incidents & alerts?**  
<span class="spoiler" tabindex="0">Alerts</span>


**Which option do you need to click to modify the incident details?**  
<span class="spoiler" tabindex="0">Manage incident</span>


Advanced hunting, including the shared and community query templates:

![Advanced hunting queries](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744215260650.gif)

![New query in editor](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744215577388.png)

My first advanced hunting query:

```kql
AlertInfo
| sort by AlertId
```

![AlertInfo results](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744215577806.png)

## Lab: Reviewing Custom Unified RBAC Roles

I reviewed the **Defender Analyst T1** custom role. Permissions are split into three groups: security operations, security posture, and authorization and settings.

![Defender Analyst T1 role](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797496566.png)

![Security operations permissions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797496526.png)

![Security posture permissions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797316801.png)

![Authorization and settings permissions](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797316625.png)

Each role can have several assignments, each with its own users and the Defender products it applies to.

![Edit assignment](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797316573.png)

![Assignment details](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1744797316565.png)

An L1 analyst role should really only need security operations.

**How many permission groups are available?**  
<span class="spoiler" tabindex="0">3</span>


**Which permission group is for security admins in charge of configuring security controls?**  
<span class="spoiler" tabindex="0">Authorization and settings</span>

