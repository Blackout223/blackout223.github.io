---
layout: default
title: MS Sentinel - Investigate
---

# MS Sentinel: Investigate

Working incidents as an L1 analyst: taking ownership, investigating, and closing or escalating.

## Events, alerts and incidents

- **Event:** anything that happens and gets logged. Most are normal.
- **Alert:** raised when one or more events match a rule.
- **Incident:** one or more related alerts that need someone to investigate.

Not every incident comes from Sentinel's own rules. In most environments a lot of them come from connected products like the Defender suite.

<img alt="Incidents from other products" src="https://github.com/user-attachments/assets/c63529c0-744a-4add-93e0-c5f1181be4bf" />

## Triage

The first step is to assign the incident to yourself and change the status from **New** to **Active**, so nobody else picks up the same thing.

<img alt="Changing status to Active" src="https://github.com/user-attachments/assets/e56b7859-1381-44a4-9b67-36330562c248" />

Incidents can have tasks attached. These are a checklist of steps set by senior analysts, so every analyst handles the same type of incident the same way, whatever shift they're on.

<img alt="Incident tasks" src="https://github.com/user-attachments/assets/d950b4f3-93ee-463d-a475-dae02f7cc86f" />

## Investigating

The **timeline** shows the alerts in order and can be filtered by severity or MITRE tactic.

<img alt="Incident timeline" src="https://github.com/user-attachments/assets/b4bfeb20-988d-4313-90b4-bcbdb1b3a0f4" />

**Link to LA** opens the alert's query in Log Analytics, so you can see the raw logs without leaving the incident.

<img alt="Link to Log Analytics" src="https://github.com/user-attachments/assets/275f947e-a0b8-4b89-b53e-fcaa86f101dd" />

The logs show the entities involved, like accounts, hosts and IP addresses.

<img alt="Entities in the logs" src="https://github.com/user-attachments/assets/1de2b6cc-3ec4-40c5-af8f-4298bebf23e9" />

Once you have an IP, enrichment can add context to it, for example looking up its location.

<img alt="IP geolocation" src="https://github.com/user-attachments/assets/cba34f25-ff15-489c-a0c3-2ad3e20e95fd" />

**Investigate** opens a graph of the alerts and entities and how they connect.

<img alt="Investigation graph" src="https://github.com/user-attachments/assets/3f8c35e8-c74d-425b-a75f-2cc1f7266abb" />

<img alt="Investigation graph" src="https://github.com/user-attachments/assets/a2f895ff-81bf-40a7-9b1b-7f4e80c4781f" />

Hovering over an entity gives exploration queries, which are ready-made follow-up questions like "what else has this IP done?"

<img alt="Exploration queries" src="https://github.com/user-attachments/assets/2ca1d0a1-5631-498b-b7b2-36abe9143090" />

## Closing incidents

Every closed incident gets a classification:

- **True positive:** a real threat.
- **Benign positive:** real activity, but expected. For example a pen test or red team exercise.
- **False positive:** the rule logic was wrong or the data was wrong.
- **Undetermined:** not enough information to decide.

False positives can be handled two ways. An automation rule can close matching incidents without touching the analytics rule, which is quick but doesn't fix the cause. Or the analytics rule itself can be changed, which is the proper long-term fix.

## Lab: investigating incidents

I reviewed the open incidents and checked the MITRE tactics on the Solorigate incidents.

<img alt="Solorigate incident" src="https://github.com/user-attachments/assets/debeaf02-2a5f-43e1-bc07-2886b8b17d2a" />

Then I looked at **Sign-ins from IPs that attempt sign-ins to disabled accounts**. I found the IP entity involved and looked up its location.

<img alt="IP entity" src="https://github.com/user-attachments/assets/21a8e3af-8a85-4782-ac4d-9991f1065d62" />

In the alert logs I found which disabled account was being targeted and how many times it was tried.

<img alt="Alert logs" src="https://github.com/user-attachments/assets/2c1e9ff4-c703-4ccb-ab63-432d8a73de0f" />

Sign-in attempts against a disabled account are a good signal. Nobody legitimate should be logging into an account that's been switched off, so it usually means someone is trying old or leaked credentials. Next steps would be checking whether the same IP tried any active accounts, and escalating to L2 if it had.

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### Events/Alerts/Incidents

**What is a normal activity that happens at a point in time, neither good nor bad called?**  
<span class="spoiler" tabindex="0">Event</span>

**What is a negative event that disrupts normal operations and requires investigation called?**  
<span class="spoiler" tabindex="0">Incident</span>

### Incidents Overview

**Is Microsoft Sentinel the only product contributing to incidents? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Nay</span>

### Triage and Investigate - Ownership, Status, Tasks

**What ensures the smooth and efficient operation of security operations (SecOps)?**  
<span class="spoiler" tabindex="0">process standardization</span>

**What helps to guarantee that every incident receives uniform treatment?**  
<span class="spoiler" tabindex="0">Incident Tasks</span>

### Triage and Investigate - Timeline, Logs, Entities, Visual

**Which link can you click to see the specific logs of an alert without leaving the context of the incident details page?**  
<span class="spoiler" tabindex="0">Link to LA</span>

**During graphical investigation, which queries can be utilized to deepen the investigation?**  
<span class="spoiler" tabindex="0">Exploration</span>

### Closing Incidents

**You investigate an incident and conclude that it is due to red team activities. What is your closure classification?**  
<span class="spoiler" tabindex="0">benign positive</span>

**How do you eliminate a false positive without modifying the analytics rule?**  
<span class="spoiler" tabindex="0">automation rule</span>

**What do you classify an actual threat as?**  
<span class="spoiler" tabindex="0">True Positive</span>

### Lab-05: Investigate Incidents

**What is the MITRE Tactics classification for Solorigate incidents?**  
<span class="spoiler" tabindex="0">Command and Control</span>

**Investigate the incident "Sign-ins from IPs that attempt sign-ins to disabled accounts". What is the IP entity involved in this incident?**  
<span class="spoiler" tabindex="0">175.45.176.99</span>

**Check out this IP's geolocation. What is the city?**  
<span class="spoiler" tabindex="0">Pyeongyang</span>

**Now, dive back into the alert logs for this incident. Which disabled account was targeted by attackers?**  
<span class="spoiler" tabindex="0">johns@m365x816222.onmicrosoft.com</span>

**How many login attempts were there for this disabled account?**  
<span class="spoiler" tabindex="0">4</span>
