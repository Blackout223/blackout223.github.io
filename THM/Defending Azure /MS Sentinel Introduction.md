---
layout: default
title: MS Sentinel - Introduction
---

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
