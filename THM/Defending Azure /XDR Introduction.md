---
layout: default
title: Microsoft Defender XDR - Introduction
---

# Microsoft Defender XDR: Introduction

What Defender XDR is, how its pieces fit together, and a walk round the portal.

## EDR vs XDR

EDR only sees endpoints: laptops, desktops and servers. XDR pulls in signals from endpoints, email, identities and cloud apps, and links them together. So a phishing email, the malware it dropped and the account it then abused can show up as one incident instead of three unrelated alerts in three different tools.

## What feeds into it

- **Defender for Endpoint:** EDR, antivirus and vulnerability management on devices
- **Defender for Office 365:** email, links and attachments
- **Defender for Identity:** on-prem Active Directory, catching things like lateral movement with stolen accounts
- **Defender for Cloud Apps:** cloud app usage, including apps IT hasn't approved
- **Entra ID Protection:** risky sign-ins, used together with Conditional Access

All of this lands in one incident queue at security.microsoft.com. From there you can hunt with KQL (advanced hunting), and Defender can respond automatically, for example isolating a device or disabling an account.

## Following an attack through the products

The room walks through a typical attack and which product would catch each stage:

1. A phishing email arrives. Exchange Online Protection and mail flow rules can stop it.
2. Safe Links and Safe Attachments check any links or files that get through.
3. Defender for Endpoint finds and fixes vulnerabilities before malware can use them.
4. Defender for Identity flags odd account changes and lateral movement.
5. Defender for Cloud Apps spots data being taken out, like impossible travel or unusual downloads.

This is basically defence in depth: if one layer misses it, the next one has a chance.

## Settings and roles

XDR-wide settings are under **Settings > Microsoft Defender XDR**. This includes email notifications, alert tuning, critical asset management, and excluding users from automated responses.

Access can be handled three ways:

- Entra ID roles like Security Administrator or Security Reader
- Custom roles per Defender product, under **Permissions**
- **Unified RBAC**, which manages permissions for all the Defender products in one place

Unified RBAC is the cleanest option for least privilege. It has to be switched on first, either from **Permissions > Roles** (Activate workloads or Workload settings) or from **Settings > Microsoft Defender XDR > Permissions and roles**.

## Lab: the portal and roles

I went through the main areas: the incident queue, an incident's attack story and graph, and advanced hunting with the shared and community queries.

My first advanced hunting query:

```kql
AlertInfo
| sort by AlertId
```

Then I reviewed the **Defender Analyst T1** custom role. Permissions are split into three groups: security operations, security posture, and authorization and settings. An L1 analyst role should really only need security operations.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### What is Microsoft Defender XDR

**What is EDR (Endpoint Detection and Response) visibility limited to?**  
<span class="spoiler" tabindex="0">Endpoints</span>

**Is Microsoft Defender XDR purpose-built for organizations using Microsoft products? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

### Microsoft Defender XDR Architecture

**Which of the Defender XDR signals focuses on email security?**  
<span class="spoiler" tabindex="0">Microsoft Defender for Office 365</span>

**Which querying language does Microsoft Defender XDR use for threat hunting?**  
<span class="spoiler" tabindex="0">Kusto Query Language</span>

### Microsoft Defender XDR Settings

**Monitoring risky Lateral movement is a feature of which Microsoft Defender product?**  
<span class="spoiler" tabindex="0">Defender for Identity</span>

**Where can security admins exclude users from automated response actions?**  
<span class="spoiler" tabindex="0">identity automated response</span>

### Microsoft Defender XDR Roles and Permissions

**From which tab can security admins create custom roles for specific Microsoft Defender Products?**  
<span class="spoiler" tabindex="0">Permissions</span>

### Microsoft Defender XDR Unified RBAC

**How many ways can Microsoft Defender XDR unified role-based access control (RBAC) be activated?**  
<span class="spoiler" tabindex="0">2</span>

**Aside from the Activate workload option, which other option can be used to activate Microsoft Defender XDR unified role-based access control (RBAC) from the permissions and roles page?**  
<span class="spoiler" tabindex="0">workload settings</span>

### Lab: Navigating Defender XDR Portal

**Besides the Incidents and Email & collaboration alerts tabs, what other tab can be found under Incidents & alerts?**  
<span class="spoiler" tabindex="0">Alerts</span>

**Which option do you need to click to modify the incident details?**  
<span class="spoiler" tabindex="0">Manage incident</span>

### Lab: Reviewing Custom Unified RBAC Roles

**How many permission groups are available?**  
<span class="spoiler" tabindex="0">3</span>

**Which permission group is for security admins in charge of configuring security controls?**  
<span class="spoiler" tabindex="0">Authorization and settings</span>
