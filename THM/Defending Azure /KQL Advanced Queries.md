---
layout: default
title: KQL (Kusto) - Advanced Queries
---

# KQL (Kusto): Advanced Queries

*Room questions are in bold under each section. Answers are blurred: hover over or tap one to see it.*

## Using the Join Operator

`join` matches rows from two tables on a shared column, the same idea as a SQL join. The `kind` decides what you get back:

- `inner`: a row for every match between the two tables
- `innerunique` (the default): matches, but duplicates on the left are removed first
- `leftouter` / `rightouter` / `fullouter`: everything from one or both sides, with blanks where there's no match
- `leftsemi` / `rightsemi`: rows from one side that do have a match
- `leftanti` / `rightanti`: rows from one side with **no** match. Good for finding gaps.

Failed logins per computer, joined with `Heartbeat` to add each machine's environment:

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedLogins = count() by Computer
| join kind=inner (
    Heartbeat
) on Computer
| project Computer, FailedLogins, ComputerEnvironment
```

![Inner join results](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738591005246.png)

Machines sending heartbeats but with no security events, which could mean logging isn't set up properly on them:

```kql
SecurityEvent
| join kind=rightanti (
    Heartbeat
) on Computer
| project TimeGenerated, Computer, ComputerIP, ComputerEnvironment
```

![Rightanti join results](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738591005522.png)

**Which join flavor returns all the records from the left table and only matching values from the right table?**  
<span class="spoiler" tabindex="0">kind=leftouter</span>


**Which join flavor contains a row in the output for every matching row combination from both tables?**  
<span class="spoiler" tabindex="0">kind=inner</span>


## Using the Union Operator

`union` stacks the rows from several tables into one result. It's handy when the same thing could be in more than one place:

```kql
SecurityEvent
| union ProtectionStatus
```

![Union of SecurityEvent and ProtectionStatus](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738611435171.png)

It also works for putting two different summaries in one output:

```kql
SecurityEvent
| summarize count() by Computer
| union (
    SigninLogs
    | summarize count() by Location
)
| order by count_ desc
```

![Union of two summaries](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738611435387.png)

**Can the union operator return all rows from both tables? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


```kql
VMComputer
| where Computer == "CH1-UBNTVM"
| union ProtectionStatus
| where Computer == "CH1-UBNTVM"
| where ProtectionStatus != "Real Time Protection"
```

![Union filtered to one machine](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738611435570.png)

**Run the "Combining Multiple Columns From Different Tables" query and modify the time range. What is the threat status detail?**  
<span class="spoiler" tabindex="0">Threat Status is currently not supported in MDATP</span>


## Using the Project Operators

- `project` picks, renames or adds columns
- `project-away` drops columns, which is easier than listing every column you want to keep
- `project-keep` keeps columns matching a pattern
- `project-rename` and `project-reorder` tidy up the output

Sign-ins from outside the US in the last 5 days, a starting point for impossible travel:

```kql
SigninLogs
| where TimeGenerated >= ago(5d)
| where Location != "US"
| project AppDisplayName, Location, DeviceDetail, UserDisplayName, ResultDescription
```

![Sign-ins outside the US](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738611656695.png)

Failed MFA challenges (result type 50074) per user:

```kql
SigninLogs
| where ResultType == 50074
| project-keep UserDisplayName, Identity, UserPrincipalName, ResultDescription, AppDisplayName, AppId, ResourceDisplayName
| summarize FailureCount = count(), FailedResources = dcount(ResourceDisplayName), ResultDescription = any(ResultDescription) by UserDisplayName
```

![Failed MFA challenges](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738615198853.png)

![project-away results](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738615199103.png)

**Which project operator sets the column order in the resulting output?**  
<span class="spoiler" tabindex="0">project-reorder</span>


**Can project operators be used to add, remove, or rename columns in a result set? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>


## Using the Extend Operator

`extend` adds a calculated column and keeps all the others.

```kql
SecurityEvent
| where Computer == "JBOX10"
| extend TimeSinceReboot = (now() - TimeGenerated)
| project Computer, TimeGenerated, TimeSinceReboot
```

![TimeSinceReboot column](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738616178148.png)

Labelling logon and logoff events:

```kql
SecurityEvent
| extend EventCategory = case(EventID == 4624, "Logon", EventID == 4634, "Logoff", "Other")
| project TimeGenerated, AccountType, EventID, EventCategory, Computer
```

![EventCategory column](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738616178419.png)

Converting free disk space to GB:

```kql
Perf
| where CounterName == "Free Megabytes"
| extend FreeSpaceinGB = CounterValue / 1000
| project TimeGenerated, Computer, FreeSpaceinGB, CounterName, ObjectName
```

![FreeSpaceinGB column](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738616178912.png)

**Run the "Using Extend To Calculate Free Disk Spaces" query and modify the time range. What new column is created?**  
<span class="spoiler" tabindex="0">FreeSpaceinGB</span>


**What is the event category for event ID 4624?**  
<span class="spoiler" tabindex="0">Logon</span>


## Using the Parse Operator

`parse` pulls values out of a text field into their own columns. `parse-kv` does the same for key=value text, and `parse-where` drops rows that don't match. Here it's used on Kerberos service ticket requests (event 4769) to get the IP, port and service name out of the raw `EventData`:

```kql
SecurityEvent
| where EventID == 4769
| where TimeGenerated >= ago(60d)
```

![4769 events](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738684589931.png)

```kql
| parse EventData with * 'IpAddress">' IpAddress '</Data' *
| parse EventData with * 'IpPort">' IpPort '</Data' *
| parse EventData with * 'ServiceName">' ServiceName '</Data' *
| project TimeGenerated, Computer, IpAddress, IpPort, ServiceName, Activity
```

![Parsed EventData](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738684590113.png)

![Parsed columns in the output](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1738684589958.gif)

Lots of 4769 requests for different services from one account is a sign of Kerberoasting, so being able to pull these fields out quickly is useful.

**What column was parsed in our example?**  
<span class="spoiler" tabindex="0">EventData</span>


**Run the "Filter by EventID" query and modify the time range. What is the event level name?**  
<span class="spoiler" tabindex="0">6</span>


## Creating KQL Parsers

A query can be saved as a function (a parser) from **Save > Save as function**, then reused from **Functions > Workspace functions**. Useful for anything you run regularly.

```kql
SecurityEvent_CL
| where Computer contains_cs "DC"
| where TimeGenerated >= ago(30d)
```

**Under which dropdown can you find your saved function?**  
<span class="spoiler" tabindex="0">workspace functions</span>


## Lab-02: Discover

TeamViewer may have been compromised and isn't approved in the organisation, so the task was to find every Azure VM with it installed, check their pending updates, and recommend next steps.

Finding the machines:

```kql
ConfigurationData_CL
| where ConfigDataType_s == "Software"
| where Publisher_s == "TeamViewer"
| where TimeGenerated >= ago(10d)
| summarize arg_max(TimeGenerated, *) by SoftwareName_s, Computer
| project-away _ResourceId_s, Type_s
```

`arg_max(TimeGenerated, *)` keeps only the latest record for each machine, so each one only appears once.

![Machines with TeamViewer installed](/THM/images/kql/11.png)

**What table is queried to search for installed software?**  
<span class="spoiler" tabindex="0">ConfigurationData_CL</span>


![13 machines with TeamViewer](/THM/images/kql/10.png)

**How many machines have TeamViewer installed on them?**  
<span class="spoiler" tabindex="0">13</span>


Pending Windows updates that aren't optional:

```kql
Update_CL
| where OSType_s != "Linux" and UpdateState_s == "Needed" and Optional_s == "FALSE"
| where TimeGenerated >= ago(10d)
| project TimeGenerated, Computer, Title_s, KBID_s, Classification_s, MSRCSeverity_s, PublishedDate_UTC__s, _ResourceId_s
| sort by TimeGenerated desc
```

For the rest of the questions I wrote my own queries. Critical updates for one machine:

```kql
Update_CL
| where OSType_s != "Linux"
  and UpdateState_s == "Needed"
  and Optional_s == "FALSE"
  and Computer == "JBOX00"
  and MSRCSeverity_s == "Critical"
  and TimeGenerated >= ago(10d)
| summarize CriticalUpdateCount = count()
```

**How many critical updates are found for JBOX00?**  
<span class="spoiler" tabindex="0">6</span>


Unique Linux machines:

```kql
Update_CL
| where OSType_s == "Linux"
| summarize UniqueLinuxMachines = dcount(Computer)
```

**Search the Update_CL table. How many unique Linux machines can you find?**  
<span class="spoiler" tabindex="0">4</span>


Machines with Chrome installed:

```kql
ConfigurationData_CL
| where ConfigDataType_s == "Software"
| where SoftwareName_s contains "Chrome"
| summarize UniqueMachinesWithChrome = dcount(Computer)
```

**How many machines have Google Chrome installed?**  
<span class="spoiler" tabindex="0">2</span>


My recommendations were to export the list of machines with TeamViewer and send it to whoever manages devices to get it uninstalled, send the pending updates list to the patching team, and save the TeamViewer query as a function so it can be rerun to check it's been removed.
