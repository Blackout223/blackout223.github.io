---
layout: default
title: KQL (Kusto) - Introduction
---

# KQL (Kusto): Introduction

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## Overview of Microsoft Sentinel

Sentinel is Microsoft's cloud SIEM, with SOAR built in. It pulls in logs from Microsoft services like Entra ID, the Defender products and Azure Monitor, and from third-party products through data connectors. Anything that can send Syslog can be ingested, and for products with no connector there's a REST API, although that usually means writing your own script.

**In addition to being a SIEM solution, what else is Microsoft Sentinel?**  
<span class="spoiler" tabindex="0">SOAR</span>


**How does MS Sentinel support other security solutions that are not included in the built-in connectors?**  
<span class="spoiler" tabindex="0">REST API Integration</span>


## What Is KQL

KQL (Kusto Query Language) is how you get anything useful out of Sentinel. It was originally built for Azure Data Explorer and is now used in Sentinel, Azure Monitor, Log Analytics and Defender XDR's advanced hunting. Queries run against the logs stored in a Log Analytics workspace.

**What initial service was KQL created for?**  
<span class="spoiler" tabindex="0">Azure Data Explorer</span>


**Analyze the example query from the task. How many computers will the query return?**  
<span class="spoiler" tabindex="0">10</span>


**What table is the example query retrieving its data from?**  
<span class="spoiler" tabindex="0">Heartbeat</span>


## KQL Concepts in Microsoft Sentinel

Logs are stored in tables, usually one table per data source. For example `SecurityEvent` for Windows security events, `AzureActivity` for Azure activity, and `SigninLogs` for Entra ID sign-ins. Custom logs end in `_CL`.

![Custom log tables ending in _CL](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1721336368165.png)

The operators I used most:

| Operator | What it does |
|---|---|
| `where` | Filter rows |
| `project` | Pick which columns to show |
| `extend` | Add a calculated column |
| `summarize` | Count, sum or average, grouped by a column |
| `sort` / `order by` | Sort |
| `take` / `limit` | Return a set number of rows |
| `search` | Search across tables when you don't know where the data is |
| `join` / `union` | Combine tables |
| `let` | Set a variable |
| `ago()` | A time relative to now, like `ago(1h)` |
| `render` | Turn the results into a chart |

**What operator can be used to output results in graphical form?**  
<span class="spoiler" tabindex="0">Render</span>


**What operator can be used to filter a specified table based on specified conditions?**  
<span class="spoiler" tabindex="0">Where</span>


```kql
SecurityEvent
| where TimeGenerated >= ago(3h) and TargetUserName == "JBOX00$"
| project TimeGenerated, Account, Activity, Computer
| sort by TimeGenerated desc
```

**What user account name was queried in our second example query above?**  
<span class="spoiler" tabindex="0">JBOX00$</span>


## KQL Statement Structure

A query starts with a table, then each operator is joined with a pipe `|`. The data flows down through each step, a bit like piping commands together in Linux.

```kql
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize EventCount = count() by Computer
| order by EventCount desc
| limit 10
```

This takes the last day of security events, counts them per computer and shows the top 10.

**What is the name of the table queried in our example query?**  
<span class="spoiler" tabindex="0">SecurityEvent</span>


**Analyze the example query from the task. What does the query aggregate per computer?**  
<span class="spoiler" tabindex="0">EventCount</span>


## KQL Use Cases

Looking for failed logins (event ID 4625):

```kql
SecurityEvent
| where EventID == 4625
```

![Failed logon results](/THM/images/kql/01.png)

**Run the first example query from the task. What is event ID 4625?**  
<span class="spoiler" tabindex="0">An account failed to log on</span>


Then narrowing it to admin accounts:

```kql
SecurityEvent
| where EventID == 4625 and TargetUserName contains "admin"
```

![Failed logons for admin accounts](/THM/images/kql/02.png)

**Run the second example query from the task. What is the account type found?**  
<span class="spoiler" tabindex="0">User</span>


This is how I'd start looking into a possible brute force or password spray: start broad, then keep adding `where` lines until the noise is gone. KQL is also used for writing analytics rules, hunting, and building reports.
