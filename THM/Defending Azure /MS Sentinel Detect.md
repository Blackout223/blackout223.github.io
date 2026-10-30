---
layout: default
title: MS Sentinel - Detect
---

# MS Sentinel: Detect

With logs coming in, the next step is analytics rules. These run queries against the data and raise alerts when something matches. Alerts are then grouped into incidents for analysts to work on.

## Rule types

- **Scheduled:** the most common type. A KQL query that runs on a set schedule and looks back over a set period.
- **NRT (near-real-time):** runs every minute, for things that need a quick response.
- **Fusion:** Microsoft's correlation engine. It joins up lots of low-confidence alerts into one higher-confidence incident for multi-stage attacks. On by default.
- **ML Behavior Analytics:** Microsoft's own machine learning rules. You can't see or change the logic.
- **Threat Intelligence:** matches your logs against Microsoft's threat intel indicators. Can't be customised.

Rules start as templates. You create an active rule from a template and can change its query, schedule and settings along the way.

## Lab: enabling an analytics rule

I installed the **Azure Activity** solution from the Content hub, then went to **Analytics > Rule templates**.

<img alt="Rule templates" src="https://github.com/user-attachments/assets/0c029168-d8e4-47ae-9bfc-21cab94352dc" />

I searched for **Rare subscription-level operations in Azure** and selected **Create rule**.

<img alt="Searching for the rule" src="https://github.com/user-attachments/assets/7dd05e4b-eaef-457f-b4ae-0906ea577edf" />

<img alt="Create rule" src="https://github.com/user-attachments/assets/4b57b916-daaa-4629-8f3a-26be11667a87" />

The rule is mapped to MITRE ATT&CK tactics and techniques, which tells you what kind of attacker activity it's meant to catch.

<img alt="MITRE ATT&CK mapping" src="https://github.com/user-attachments/assets/38c0ab38-cdeb-40c4-89b4-d6c5485be105" />

On **Set rule logic**, the query looks for operations in a list called `SensitiveOperationList`. To tune the rule for a different environment you'd just add or remove operations in that list.

<img alt="Rule query" src="https://github.com/user-attachments/assets/42e4f4fb-b822-4699-8eef-9e9835204513" />

**Test with current data** shows how often the rule would have fired on the existing logs. This is useful for spotting a noisy rule before you turn it on.

<img alt="Test with current data" src="https://github.com/user-attachments/assets/37cd93e1-1cf2-46ff-be7b-ae449103b7fb" />

I set it to run every hour and look back over the last 7 days.

<img alt="Query scheduling" src="https://github.com/user-attachments/assets/c2b788e2-6e37-4030-9fb0-250233378b9c" />

I left incident creation on. This is also where you'd group alerts so one problem doesn't create a pile of separate incidents.

<img alt="Incident settings" src="https://github.com/user-attachments/assets/815c1302-0716-4006-96d9-5de4ab3f9ab1" />

After saving, I edited the rule from the **Active rules** tab to add an automation rule.

<img alt="Editing the active rule" src="https://github.com/user-attachments/assets/b80f9361-8479-4ec6-94c4-d8c065cda2ef" />

The automation rule adds the tag `subscription` to every incident this rule creates. That makes them easy to filter on later.

<img alt="Automation rule" src="https://github.com/user-attachments/assets/2804cf20-0a46-4c06-acad-ea5b83eaab74" />

<img alt="Review and create" src="https://github.com/user-attachments/assets/9728b9d8-c560-4910-8569-d01bcbaa1444" />

The rule details show my schedule settings, and **Compare with template** shows exactly what I changed from the original template in YAML.

<img alt="Rule details" src="https://github.com/user-attachments/assets/eadabac0-cdf7-4b9d-b4b2-b63bc28cfc58" />

<img alt="Compare with template" src="https://github.com/user-attachments/assets/551def4d-5752-4a7b-82e7-87b871dda1a1" />

## Thoughts

Being able to compare an active rule against its template is useful. Once rules get tuned over time, it's easy to lose track of what was changed and why, and this gives you a diff. In a real setup I'd want the rules kept in Git as well, so changes get reviewed before they go live.
