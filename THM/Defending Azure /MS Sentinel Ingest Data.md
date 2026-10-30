---
layout: default
title: MS Sentinel - Ingest Data
---

# MS Sentinel: Ingest Data

Getting logs into Sentinel. Without data there's nothing to detect on.

## Data connectors and the Content hub

Logs come in through data connectors, and each connector is specific to one data source, for example Entra ID, Defender or AWS. Some connectors are built in, but most come from the Content hub.

<img alt="Data connectors" src="https://github.com/user-attachments/assets/d604049f-9ec7-4e15-acf2-092d1f213fd1" />

The Content hub is where you install packaged content. A **solution** is a bundle that can include data connectors, analytics rules, workbooks and playbooks for one product. **Standalone** content is a single item on its own. If no solutions are installed, there are no connectors to connect.

<img alt="Content hub" src="https://github.com/user-attachments/assets/e6660c4a-7647-4155-ae42-01cce6f1f140" />

<img alt="Standalone content and solutions" src="https://github.com/user-attachments/assets/2a858830-4070-414b-839e-985017fbd634" />

<img alt="Solutions" src="https://github.com/user-attachments/assets/2ce21e1f-36b6-477c-8131-7c83e378a91e" />

## Lab: installing a Content hub solution

I was logged in with the Sentinel Contributor and Log Analytics Contributor roles.

<img alt="Lab roles" src="https://github.com/user-attachments/assets/935472d2-4960-4d83-bc00-08021d1889ae" />

Under **Content management > Content hub**, nothing was installed yet.

<img alt="Content hub" src="https://github.com/user-attachments/assets/ccaf2abf-f0b2-4a93-bfd2-7c95cbcdefc4" />

<img alt="Nothing installed" src="https://github.com/user-attachments/assets/4273d7fe-76bb-4335-8284-c375930191b2" />

I searched for **Microsoft Entra ID** and installed it.

<img alt="Installing Microsoft Entra ID" src="https://github.com/user-attachments/assets/b245cd82-d5c0-4a22-b0f2-65231b881ce1" />

The solution details show what it deploys: 63 analytics rules, 1 data connector, 11 playbooks and 2 workbooks.

<img alt="Solution details" src="https://github.com/user-attachments/assets/9c8563c0-adde-4d61-b3a6-fb033c4d1cf7" />

**Manage** shows each piece of content in the package, and the connector's status.

<img alt="Package details" src="https://github.com/user-attachments/assets/d8a03e6a-3a60-4015-b41a-e178311bc8f6" />

<img alt="Data connector status" src="https://github.com/user-attachments/assets/bd3e7ac7-c812-491e-98ba-8b43fd04cf6b" />

## Connecting a data connector

Installing the solution only adds the connector. It still has to be connected. Each connector has its own page with a description, the log tables it fills, the permissions it needs and the setup steps.

<img alt="Connector page" src="https://github.com/user-attachments/assets/dbebaa17-58f4-465b-a778-f993c582409a" />

## Lab: connecting threat intelligence

I installed the **Threat Intelligence** solution.

<img alt="Installing Threat Intelligence" src="https://github.com/user-attachments/assets/f087cbd5-e83d-40ed-ac29-5f6b05817cf9" />

Then opened **Manage** to see the connectors it came with.

<img alt="Threat intelligence connectors" src="https://github.com/user-attachments/assets/7b5769fb-9815-4613-8b97-f8ab1f998741" />

I opened the **Microsoft Defender Threat Intelligence** connector. It only needs workspace-level permissions, which I already had. Other connectors can need permissions in other places, for example on the source system itself.

<img alt="MDTI connector page" src="https://github.com/user-attachments/assets/8109a79e-523f-4ac2-bc90-31d14dbaed44" />

I clicked **Connect** to start importing indicators of compromise (IPs, domains, URLs and file hashes) from Microsoft's threat intel into Sentinel, so they can be matched against our own logs.

<img alt="Connected" src="https://github.com/user-attachments/assets/bd31f8cf-7d56-4138-b6fb-250fac419b45" />

## Room questions

Answers are blurred. Hover over or tap an answer to see it.

### Getting Ready to Ingest Data

**What is used to ingest log data into Microsoft Sentinel?**  
<span class="spoiler" tabindex="0">Data Connectors</span>

### Data Connectors Introduction

**Are data connectors specific to each data source? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>

**Data connectors are available in how many ways?**  
<span class="spoiler" tabindex="0">2</span>

### Content Hub Introduction

**What are bundles of data connectors, workbooks, analytic rules, and playbooks called in Microsoft Sentinel?**  
<span class="spoiler" tabindex="0">Solutions</span>

**Content source can either be a Solution or?**  
<span class="spoiler" tabindex="0">Standalone</span>

### Lab-02: Install Content Hub Solutions

**What Content source type is Microsoft Entra ID?**  
<span class="spoiler" tabindex="0">Solution</span>

**What category is the Microsoft Entra ID?**  
<span class="spoiler" tabindex="0">Identity, Security - Automation (SOAR)</span>

### Connecting Data Connectors

**Where can you find all the details about a connector?**  
<span class="spoiler" tabindex="0">Connector Page</span>

**In order to configure the Microsoft Entra ID data connector, what permissions are required on the Log Analytics workspace?**  
<span class="spoiler" tabindex="0">Read and Write</span>

### Lab-03: Connect and Configure a Data Connector

**What does the Microsoft Defender Threat Intelligence (MDTI) connector import?**  
<span class="spoiler" tabindex="0">Indicators of compromise</span>

**Can threat indicators include IP addresses and domains? (Yea/Nay)**  
<span class="spoiler" tabindex="0">Yea</span>
