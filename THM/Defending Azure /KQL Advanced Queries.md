---
layout: default
title: KQL (Kusto) - Advanced Queries
---

# KQL (Kusto): Advanced Queries

Combining tables, reshaping output, and pulling fields out of raw text. The lab uses these to hunt for unapproved software and missing updates.

## join

`join` matches rows from two tables on a shared column, the same idea as a SQL join. The `kind` decides what you get back. The ones I found most useful:

- `inner`: only rows that match in both tables
- `leftouter`: everything from the left table, plus matches from the right
- `leftanti` / `rightanti`: rows in one table with **no** match in the other. Good for finding gaps.

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

And machines that are sending heartbeats but have no security events at all, which could mean logging isn't set up properly on them:

```kql
SecurityEvent
| join kind=rightanti (
    Heartbeat
) on Computer
| project TimeGenerated, Computer, ComputerIP, ComputerEnvironment
```

## union

`union` stacks the rows from several tables into one result. It's handy when the same thing could be in more than one place:

```kql
SecurityEvent
| union ProtectionStatus
```

## project variants

- `project-away` drops columns, which is easier than listing every column you want to keep
- `project-keep` keeps columns matching a pattern
- `project-rename` and `project-reorder` tidy up the output

Failed MFA challenges (result type 50074) per user:

```kql
SigninLogs
| where ResultType == 50074
| summarize FailureCount = count(), FailedResources = dcount(ResourceDisplayName) by UserDisplayName
```

## extend

`extend` adds a calculated column and keeps all the others. Labelling logon and logoff events:

```kql
SecurityEvent
| extend EventCategory = case(EventID == 4624, "Logon", EventID == 4634, "Logoff", "Other")
| project TimeGenerated, AccountType, EventID, EventCategory, Computer
```

## parse

`parse` pulls values out of a text field into their own columns. Here it's used on Kerberos service ticket requests (event 4769) to get the IP, port and service name out of the raw `EventData` XML:

```kql
SecurityEvent
| where EventID == 4769
| parse EventData with * 'IpAddress">' IpAddress '</Data' *
| parse EventData with * 'IpPort">' IpPort '</Data' *
| parse EventData with * 'ServiceName">' ServiceName '</Data' *
| project TimeGenerated, Computer, IpAddress, IpPort, ServiceName, Activity
```

Lots of 4769 requests for different services from one account is a sign of Kerberoasting, so being able to pull these fields out quickly is useful.

A query like this can be saved as a function (a parser) from **Save > Save as function**, then reused from **Functions > Workspace functions**.

## Lab: finding TeamViewer

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

Then pending Windows updates that aren't optional:

```kql
Update_CL
| where OSType_s != "Linux" and UpdateState_s == "Needed" and Optional_s == "FALSE"
| where TimeGenerated >= ago(10d)
| project TimeGenerated, Computer, Title_s, KBID_s, Classification_s, MSRCSeverity_s
| sort by TimeGenerated desc
```

For the room questions I wrote my own queries. Critical updates for one machine:

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

Unique Linux machines:

```kql
Update_CL
| where OSType_s == "Linux"
| summarize UniqueLinuxMachines = dcount(Computer)
```

Machines with Chrome installed:

```kql
ConfigurationData_CL
| where ConfigDataType_s == "Software"
| where SoftwareName_s contains "Chrome"
| summarize UniqueMachinesWithChrome = dcount(Computer)
```

My recommendations were to export the list of machines with TeamViewer and send it to whoever manages devices to get it uninstalled, send the pending updates list to the patching team, and save the TeamViewer query as a function so it can be rerun to check it's been removed.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### Using the Join Operator

**Which join flavor returns all the records from the left table and only matching values from the right table?**  
<span class="spoiler" tabindex="0">kind=leftouter</span>

**Which join flavor contains a row in the output for every matching row combination from both tables?**  
<span class="spoiler" tabindex="0">kind=inner</span>

### Using the Union Operator

**Can the union operator return all rows from both tables? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

**Run the "Combining Multiple Columns From Different Tables" query and modify the time range. What is the threat status detail?**  
<span class="spoiler" tabindex="0">Threat Status is currently not supported in MDATP</span>

### Using the Project Operators

**Which project operator sets the column order in the resulting output?**  
<span class="spoiler" tabindex="0">project-reorder</span>

**Can project operators be used to add, remove, or rename columns in a result set? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

### Using the Extend Operator

**Run the "Using Extend To Calculate Free Disk Spaces" query and modify the time range. What new column is created?**  
<span class="spoiler" tabindex="0">FreeSpaceinGB</span>

**What is the event category for event ID 4624?**  
<span class="spoiler" tabindex="0">Logon</span>

### Using the Parse Operator

**What column was parsed in our example?**  
<span class="spoiler" tabindex="0">EventData</span>

**Run the "Filter by EventID" query and modify the time range. What is the event level name?**  
<span class="spoiler" tabindex="0">6</span>

### Creating KQL Parsers

**Under which dropdown can you find your saved function?**  
<span class="spoiler" tabindex="0">workspace functions</span>

### Lab-02: Discover

**What table is queried to search for installed software?**  
<span class="spoiler" tabindex="0">ConfigurationData_CL</span>

**How many machines have TeamViewer installed on them?**  
<span class="spoiler" tabindex="0">13</span>

**How many critical updates are found for JBOX00?**  
<span class="spoiler" tabindex="0">6</span>

**Search the Update_CL table. How many unique Linux machines can you find?**  
<span class="spoiler" tabindex="0">4</span>

**How many machines have Google Chrome installed?**  
<span class="spoiler" tabindex="0">2</span>
