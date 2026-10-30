---
layout: default
title: KQL (Kusto) - Basic Queries
---

# KQL (Kusto): Basic Queries

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## SQL and KQL Comparison

If you know a bit of SQL, most of KQL maps across. The main difference is that KQL reads top to bottom: you start with the table and pipe it through each step.

| SQL | KQL |
|---|---|
| `SELECT * FROM t` | `t` |
| `SELECT a, b FROM t` | `t \| project a, b` |
| `SELECT * FROM t WHERE x` | `t \| where x` |
| `SELECT TOP 10 * FROM t` | `t \| take 10` |
| `SELECT COUNT(*) ... GROUP BY name` | `t \| summarize count() by name` |
| `WHERE timestamp > getdate()-1` | `t \| where TimeGenerated > ago(1d)` |

**What is the purpose of the query "table_name \| where condition"?**  
<span class="spoiler" tabindex="0">Filtering Data</span>


## Using the Where and Sort Operators

`where` is the one you use most. It can compare values, combine conditions with `and` / `or`, and use functions like `contains()` and `startswith()`. If no time is set in the query, it uses the time range picker at the top of the editor.

```kql
SecurityEvent
| where Computer == "DC11.na.contosohotels.com"
```

![Security events for DC11](/THM/images/kql/03.png)

**Run the "Finding All Security Events for a Particular Computer" query and modify the time range. What activity is EventID 4688?**  
<span class="spoiler" tabindex="0">A new process has been created</span>


`sort` and `order by` do the same thing:

```kql
VMConnection
| where Computer == "DC11.na.contosohotels.com"
| where RemoteCountry == "France"
| order by ProcessName asc
```

![Connections from DC11 to France](/THM/images/kql/04.png)

**Run the "Using the Order By Operator to Sort a Query Output" query and modify the time range. What is the source IP for the computer DC11.na.contosohotels.com?**  
<span class="spoiler" tabindex="0">10.2.2.21</span>


## Using the Search Operator

`search` looks across every table, or a list of tables, for a value. It's slower than `where`, but useful when you don't know which table something is in, like checking where an IP shows up:

```kql
search "13.89.179.10"
```

**What operator can you use if you're unsure which table or column to filter?**  
<span class="spoiler" tabindex="0">search</span>


**What suffix do custom logs have?**  
<span class="spoiler" tabindex="0">_CL</span>


## Using Let Statement for Declaring Variables

`let` sets a variable, which keeps longer queries readable and easy to reuse:

```kql
let eventIDs = dynamic([4624, 4625]);
SecurityEvent
| where EventID in (eventIDs)
| sort by TimeGenerated
```

**Which operator can you use to declare variables?**  
<span class="spoiler" tabindex="0">Let</span>


## Using Time Functions

`now()`, `ago()`, `startofday()`, `endofday()` and `datetime()` come up all the time. For example, failed logons outside 17:00 to 22:00:

```kql
SecurityEvent
| where EventID == "4625"
| where hourofday(TimeGenerated) !between (17 .. 22)
```

**Run the "Defining a Specific Time Range Within a Query" query and modify the time range. What event id are we looking for?**  
<span class="spoiler" tabindex="0">4625</span>


**Which time function returns the current UTC date and time when the query is executed?**  
<span class="spoiler" tabindex="0">now()</span>


## Using the Summarize and Render Operator

`summarize` groups and counts:

```kql
SecurityEvent
| summarize count() by Activity
```

![Count of security events by activity](/THM/images/kql/05.png)

**Run the "Using Summarize To Output the Count of Different Activities" query and modify the time range. What activity is 4768?**  
<span class="spoiler" tabindex="0">A Kerberos authentication ticket (TGT) was requested</span>


`render` turns the result into a chart, and has to be the last line of the query:

```kql
SecurityEvent
| summarize count() by Computer
| render piechart
```

**Does the render operator have to be the last operator in your KQL query? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


## Lab-01: Investigate and Identify

An alert came in for unauthorised access attempts from an account called `timadmin`. The job was to look at its activity and authentication events.

First, everything for that account:

```kql
SecurityEvent_CL
| where Account_s contains_cs "timadmin"
```

![timadmin events](/THM/images/kql/06.png)

**What is the event ID for the logged off event?**  
<span class="spoiler" tabindex="0">4634</span>


Further down the results there were 4672 events, which means the account logged on with admin rights:

![4672 special privileges events](/THM/images/kql/07.png)

**What event can be seen in the security log for timadmin relating to special privileges?**  
<span class="spoiler" tabindex="0">4672 - Special privileges assigned to new logon.</span>


Then just the logon and logoff events, counted per computer to see which machines the account was used on:

```kql
SecurityEvent_CL
| where Account_s contains_cs "timadmin"
| where Activity_s contains_cs "logged on" or Activity_s contains_cs "logged off"
| summarize count() by Computer
| order by Computer
```

![Logon and logoff count per computer](/THM/images/kql/08.png)

**What is the count of log on and log off events for DC10 by timadmin in the last 7 days?**  
<span class="spoiler" tabindex="0">21</span>


![Kerberos authentication on successful logons](/THM/images/kql/09.png)

**What is the authentication package name for timadmin's successful login?**  
<span class="spoiler" tabindex="0">Kerberos</span>


For a suspected compromised admin account, the next steps would be resetting the password, revoking sessions, and checking what it did on each machine it logged into.
