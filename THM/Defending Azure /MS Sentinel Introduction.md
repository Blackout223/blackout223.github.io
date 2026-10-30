---
layout: default
title: MS Sentinel - Introduction
---

[← Back to Defending Azure](/THM/defending-azure)

# MS Sentinel: Introduction

First room in the Defending Azure path. No lab in this one, it's the background for the rest of the Sentinel rooms, so these are my notes.

## Where Sentinel fits

A SOC's job is to find and deal with attacks. Level 1 analysts spend most of their time monitoring and triaging alerts, Level 2 handle incident response and hunting, and Level 3 do the deeper threat hunting, threat intel and vulnerability work.

Sentinel is Microsoft's cloud SIEM, with SOAR built in:

- SIEM side: collect logs, query them, correlate them and raise alerts and incidents
- SOAR side: playbooks that automate the response

## How it works

The room breaks it into four stages:

1. **Collect.** Data connectors pull logs in from different sources. The logs are stored in a Log Analytics workspace and queried with KQL.
2. **Detect.** Analytics rules run queries on a schedule and raise alerts. Workbooks are the dashboards. There are also built-in hunting queries.
3. **Investigate.** Alerts are grouped into incidents. An analyst takes ownership, works through the timeline and entities, and closes it with a classification.
4. **Respond.** Playbooks and automation rules handle repetitive work like enrichment, tagging and assigning, to cut down alert fatigue.

## Compared to what I use

At work I use Elastic SIEM, and most of this maps across: data connectors are like Elastic integrations, the Log Analytics workspace is where the data lives, and analytics rules are detection rules. Worth knowing that KQL means something different in each. In Sentinel it's Kusto Query Language, which is a full query language with pipes. In Elastic, KQL is Kibana Query Language, which is mostly for filtering.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### Microsoft Security Operations Analyst

**What security unit is responsible for protecting the organization against security threats?**  
<span class="spoiler" tabindex="0">Security Operations Center</span>

**Generally, which level of SOC Analyst is responsible for responding to incidents?**  
<span class="spoiler" tabindex="0">SOC Level 2 Analyst</span>

**Besides monitoring, what else do SOC Level 1 Analysts spend the majority of their time with?**  
<span class="spoiler" tabindex="0">Triage</span>

### Introduction to Microsoft Sentinel

**Microsoft Sentinel is a combination of two security concepts, namely SIEM and which other one?**  
<span class="spoiler" tabindex="0">SOAR</span>

**Creating security alerts and incidents is part of which security concept?**  
<span class="spoiler" tabindex="0">SIEM</span>

**By means of how many pillars does Microsoft Sentinel help us to perform security operations?**  
<span class="spoiler" tabindex="0">4</span>

### How Microsoft Sentinel Works

**What is used to ingest data into Sentinel?**  
<span class="spoiler" tabindex="0">Data Connectors</span>

**Where are the ingested logs stored for further correlation and analysis?**  
<span class="spoiler" tabindex="0">log analytics workspaces</span>

**Workbooks are essentially `_______` used for visualization**  
<span class="spoiler" tabindex="0">dashboards</span>

**When SOC teams are flooded with security alerts and incidents, this is called?**  
<span class="spoiler" tabindex="0">alert fatigue</span>

**In Microsoft Sentinel, automation is done via automated workflows, known as?**  
<span class="spoiler" tabindex="0">playbooks</span>

**The output of running Analytics rules includes security alerts and?**  
<span class="spoiler" tabindex="0">Incidents</span>

### When To Use Microsoft Sentinel

**Organizations use Microsoft Sentinel mainly because they need to `_______` their cloud infrastructure.**  
<span class="spoiler" tabindex="0">monitor</span>

**With Microsoft Sentinel, there is no need for server provisioning. This means it is?**  
<span class="spoiler" tabindex="0">cloud-native</span>
