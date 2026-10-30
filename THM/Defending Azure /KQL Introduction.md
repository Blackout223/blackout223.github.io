---
layout: default
title: KQL (Kusto) - Introduction
---

# KQL (Kusto): Introduction

KQL (Kusto Query Language) is how you get anything useful out of Sentinel. It was originally built for Azure Data Explorer and is now used in Sentinel, Azure Monitor, Log Analytics and Defender XDR's advanced hunting.

## How Sentinel gets its data

Sentinel pulls in logs from Microsoft services (Entra ID, the Defender products, Azure Monitor) and from third-party products through data connectors. Anything that can send Syslog can be ingested, and for products with no connector there's a REST API, although that usually means writing your own script.

## Tables

Everything is stored in tables in the Log Analytics workspace, usually one table per data source. For example:

- `SecurityEvent` for Windows security events
- `AzureActivity` for Azure activity logs
- `SigninLogs` for Entra ID sign-ins
- `Heartbeat` for agent check-ins from machines

Custom logs end in `_CL`, for example `SecurityEvent_CL`.

## Query structure

A query starts with a table, then each operator is joined with a pipe `|`. The data flows left to right, getting filtered or reshaped at each step, a bit like piping commands together in Linux.

```kql
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize EventCount = count() by Computer
| order by EventCount desc
| limit 10
```

This takes the last day of security events, counts them per computer and shows the top 10.

## Operators I used most

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

## A real use case

Looking for failed logins (event ID 4625), then narrowing it to admin accounts in the last hour:

```kql
SecurityEvent
| where EventID == 4625 and TargetUserName contains "admin"
| where TimeGenerated > ago(1h)
```

This is the kind of query I'd start with when looking into a possible brute force or password spray: start broad, then keep adding `where` lines until the noise is gone.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### Overview of Microsoft Sentinel

**In addition to being a SIEM solution, what else is Microsoft Sentinel?**  
<span class="spoiler" tabindex="0">SOAR</span>

**How does MS Sentinel support other security solutions that are not included in the built-in connectors?**  
<span class="spoiler" tabindex="0">REST API Integration</span>

### What Is KQL

**What initial service was KQL created for?**  
<span class="spoiler" tabindex="0">Azure Data Explorer</span>

**Analyze the example query from the task. How many computers will the query return?**  
<span class="spoiler" tabindex="0">10</span>

**What table is the example query retrieving its data from?**  
<span class="spoiler" tabindex="0">Heartbeat</span>

### KQL Concepts in Microsoft Sentinel

**What operator can be used to output results in graphical form?**  
<span class="spoiler" tabindex="0">Render</span>

**What operator can be used to filter a specified table based on specified conditions?**  
<span class="spoiler" tabindex="0">Where</span>

**What user account name was queried in our second example query above?**  
<span class="spoiler" tabindex="0">JBOX00$</span>

### KQL Statement Structure

**What is the name of the table queried in our example query?**  
<span class="spoiler" tabindex="0">SecurityEvent</span>

**Analyze the example query from the task. What does the query aggregate per computer?**  
<span class="spoiler" tabindex="0">EventCount</span>

### KQL Use Cases

**Run the first example query from the task. What is event ID 4625?**  
<span class="spoiler" tabindex="0">An account failed to log on</span>

**Run the second example query from the task. What is the account type found?**  
<span class="spoiler" tabindex="0">User</span>
