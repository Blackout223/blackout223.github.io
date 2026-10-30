---
layout: default
title: KQL (Kusto) - Basic Queries
---

# KQL (Kusto): Basic Queries

The core operators for filtering, searching and summarising logs, finishing with a lab investigating a possibly compromised admin account.

## Coming from SQL

If you know a bit of SQL, most of KQL maps across. The main difference is that KQL reads top to bottom: you start with the table and pipe it through each step.

| SQL | KQL |
|---|---|
| `SELECT * FROM t` | `t` |
| `SELECT a, b FROM t` | `t \| project a, b` |
| `SELECT * FROM t WHERE x` | `t \| where x` |
| `SELECT TOP 10 * FROM t` | `t \| take 10` |
| `SELECT COUNT(*) ... GROUP BY name` | `t \| summarize count() by name` |
| `WHERE timestamp > getdate()-1` | `t \| where TimeGenerated > ago(1d)` |

## where and sort

`where` is the one you use most. It can compare values, combine conditions with `and` / `or`, and use functions like `contains()` and `startswith()`.

```kql
SecurityEvent
| where EventID == 4625
| sort by Account asc
```

Failed logons, sorted by account. If no time is set in the query, it uses the time range picker at the top of the editor.

## search

`search` looks across every table, or a list of tables, for a value. It's slower than `where`, but useful when you don't know which table something is in, like checking where an IP shows up:

```kql
search "13.89.179.10"
```

## let

`let` sets a variable, which keeps longer queries readable and makes them easy to reuse:

```kql
let eventIDs = dynamic([4624, 4625]);
SecurityEvent
| where EventID in (eventIDs)
| sort by TimeGenerated
```

## Time functions

`now()`, `ago()`, `startofday()`, `endofday()` and `datetime()` are used all the time. For example, failed logons outside 17:00 to 22:00:

```kql
SecurityEvent
| where EventID == "4625"
| where hourofday(TimeGenerated) !between (17 .. 22)
```

## summarize and render

`summarize` groups and counts, and `render` turns the result into a chart. `render` has to be the last line of the query.

```kql
SecurityEvent
| summarize count() by Computer
| render piechart
```

## Lab: investigating timadmin

An alert came in for unauthorised access attempts from an account called `timadmin`. The job was to look at its activity and authentication events.

First, everything for that account in the custom security log table:

```kql
SecurityEvent_CL
| where Account_s contains_cs "timadmin"
```

Then just the logon and logoff events:

```kql
SecurityEvent_CL
| where Account_s contains_cs "timadmin"
| where Activity_s contains_cs "logged on" or Activity_s contains_cs "logged off"
```

And counted per computer, to see which machines the account was being used on:

```kql
SecurityEvent_CL
| where Account_s contains_cs "timadmin"
| where Activity_s contains_cs "logged on" or Activity_s contains_cs "logged off"
| summarize count() by Computer
| order by Computer
```

The logs showed a 4672 event (special privileges assigned to a new logon), which means the account logged on with admin rights. For a suspected compromised admin account, the next steps would be resetting the password, revoking sessions, and checking what it did on each machine it logged into.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### SQL and KQL Comparison

**What is the purpose of the query "table_name | where condition"?**  
<span class="spoiler" tabindex="0">Filtering Data</span>

### Using the Where and Sort Operators

**Run the "Finding All Security Events for a Particular Computer" query and modify the time range. What activity is EventID 4688?**  
<span class="spoiler" tabindex="0">A new process has been created</span>

**Run the "Using the Order By Operator to Sort a Query Output" query and modify the time range. What is the source IP for the computer DC11.na.contosohotels.com?**  
<span class="spoiler" tabindex="0">10.2.2.21</span>

### Using the Search Operator

**What operator can you use if you're unsure which table or column to filter?**  
<span class="spoiler" tabindex="0">search</span>

**What suffix do custom logs have?**  
<span class="spoiler" tabindex="0">_CL</span>

### Using Let Statement for Declaring Variables

**Which operator can you use to declare variables?**  
<span class="spoiler" tabindex="0">Let</span>

### Using Time Functions

**Run the "Defining a Specific Time Range Within a Query" query and modify the time range. What event id are we looking for?**  
<span class="spoiler" tabindex="0">4625</span>

**Which time function returns the current UTC date and time when the query is executed?**  
<span class="spoiler" tabindex="0">now()</span>

### Using the Summarize and Render Operator

**Run the "Using Summarize To Output the Count of Different Activities" query and modify the time range. What activity is 4768?**  
<span class="spoiler" tabindex="0">A Kerberos authentication ticket (TGT) was requested</span>

**Does the render operator have to be the last operator in your KQL query? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

### Lab-01: Investigate and Identify

**What is the event ID for the logged off event?**  
<span class="spoiler" tabindex="0">4634</span>

**What event can be seen in the security log for timadmin relating to special privileges?**  
<span class="spoiler" tabindex="0">4672 - Special privileges assigned to new logon.</span>

**What is the count of log on and log off events for DC10 by timadmin in the last 7 days?**  
<span class="spoiler" tabindex="0">21</span>

**What is the authentication package name for timadmin's successful login?**  
<span class="spoiler" tabindex="0">Kerberos</span>
