---
layout: default
title: Defending Azure - TryHackMe
---

[← Back to TryHackMe write-ups](/thm)

# Defending Azure

My notes from TryHackMe's Defending Azure path, which covers Microsoft's security tools: Sentinel for SIEM, KQL for querying logs, and Defender XDR for investigating and responding to attacks. The room questions are on each page, with the answers blurred.

## Microsoft Sentinel

| Module | Description | Difficulty |
|--------|-------------|------------|
| [MS Sentinel Introduction](/THM/Defending%20Azure%20/MS%20Sentinel%20Introduction) | What is Microsoft Sentinel and how does it fit into the role of a Security Operations Analyst? | Easy |
| [MS Sentinel Deploy](/THM/Defending%20Azure%20/MS%20Sentinel%20Deploy) | All the things you need for the initial Microsoft Sentinel deployment | Easy |
| [MS Sentinel Ingest Data](/THM/Defending%20Azure%20/MS%20Sentinel%20Ingest%20Data) | No logs, no correlation, no analysis, no action. Where is my log data? | Easy |
| [MS Sentinel Detect](/THM/Defending%20Azure%20/MS%20Sentinel%20Detect) | This room showcases how MS Sentinel helps in detecting and investigating threats | Easy |
| [MS Sentinel Investigate](/THM/Defending%20Azure%20/MS%20Sentinel%20Investigate) | Investigate and manage incidents in Microsoft Sentinel | Medium |

## KQL

| Module | Description |
|--------|-------------|
| [KQL (Kusto): Introduction](/THM/Defending%20Azure%20/KQL%20Introduction) | What KQL is, how Sentinel stores data in tables, and how a query is put together |
| [KQL (Kusto): Basic Queries](/THM/Defending%20Azure%20/KQL%20Basic%20Queries) | where, search, let, time functions, summarize and render, plus a lab investigating a suspicious admin account |
| [KQL (Kusto): Advanced Queries](/THM/Defending%20Azure%20/KQL%20Advanced%20Queries) | join, union, project, extend and parse, plus a lab hunting for unapproved software and missing updates |

## Microsoft Defender XDR

| Module | Description |
|--------|-------------|
| [Defender XDR: Introduction](/THM/Defending%20Azure%20/XDR%20Introduction) | How Defender XDR links endpoint, email, identity and cloud signals, plus portal and RBAC labs |
| [Defender XDR: Execution](/THM/Defending%20Azure%20/XDR%20Execution) | Malicious PowerShell and scheduled tasks, and investigating a suspicious script alert |
| [Defender XDR: Defense Evasion](/THM/Defending%20Azure%20/XDR%20Defense%20Evasion) | Impair defenses and masquerading, and investigating an attempt to turn off Defender |
| [Defender XDR: Privilege Escalation](/THM/Defending%20Azure%20/XDR%20Privilege%20Escalation) | UAC bypass and investigating a privilege escalation incident |
| [Defender XDR: Credential Access](/THM/Defending%20Azure%20/XDR%20Credential%20Access) | Brute force and credential dumping, with hunting queries for LSASS and NTDS access |
| [Defender XDR: Lateral Movement](/THM/Defending%20Azure%20/XDR%20Lateral%20Movement) | WinLNK malware and Mimikatz, and working through a multi-stage incident |
