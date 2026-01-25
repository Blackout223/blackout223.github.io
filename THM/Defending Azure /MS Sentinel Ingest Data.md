
# Getting Ready to Ingest Data

The next logical phase is to plan and execute the log data ingestion process. In Microsoft Sentinel, logs are sent to Log Analytics workspaces via data connectors.

 The main parts of this room will be:

- **Data connectors**
- **Content hub** solutions  
- How to **install** Content hub solutions
- How to **connect** data connectors

**What is used to ingest log data into Microsoft Sentinel?**
**A: Data Connectors**

# Data Connectors Introduction

## Data Sources and Data connectors

Data connectors are used to connect log data to various data sources.

There are several hundreds of associating data connectors since these connectors are specific to the data sources, data types and data formats of the ingested log data.

Sentinel includes various data connectors for both Microsoft and third-party products to assist in onboarding your data.

These data connectors are essential for integrating and ingesting data from various sources, such as Microsoft Defender, AWS, Cisco, etc., into Microsoft Sentinel, providing real-time threat detection and analysis.

**Data connectors** are available in various ways:

- Out-of-the-box (OOTB)
- Packaged with Content hub solutions

The Content hub provides a packaged way of installing and enabling data connectors for various data sources.

<img width="1307" height="795" alt="image" src="https://github.com/user-attachments/assets/d604049f-9ec7-4e15-acf2-092d1f213fd1" />

**Are data connectors specific to each data source? (Yea/Nay)**
**A: Yea**

**Data connectors are available in how many ways?**
**A: 2**

# Content Hub Introduction

**Content hub** is a place to centrally discover, install, enable, and manage out-of-the-box content and solutions for Microsoft Sentinel.

Content hub provides:

- Centralized in-product search for solutions
- Single-step deployment
- Installation of out-of-the-box solutions and content in Microsoft Sentinel

Can be referred to as a SIEM library that enables you to ingest data, monitor, alert, hunt, investigate, respond, and connect with different products, platforms, and services in Microsoft Sentinel.

<img width="1533" height="772" alt="image" src="https://github.com/user-attachments/assets/e6660c4a-7647-4155-ae42-01cce6f1f140" />

## Content Source: Standalone vs Solutions

Content sources can be **standalone** content or **solutions**.

<img width="293" height="175" alt="image" src="https://github.com/user-attachments/assets/2a858830-4070-414b-839e-985017fbd634" />

Microsoft Sentinel Solutions are packages of content, such as data connectors, workbooks, analytic rules, playbooks, etc., that fulfil an end-to-end product, domain, or industry vertical scenario in Microsoft Sentinel. We will explore the details of these content types in some upcoming rooms, but for now, we need to get a good grasp of how they are organized in Microsoft Sentinel.

<img width="302" height="338" alt="image" src="https://github.com/user-attachments/assets/2ce21e1f-36b6-477c-8131-7c83e378a91e" />

 With no Content hub solution installed, you won't see any data connectors available to connect.

**What are bundles of data connectors, workbooks, analytic rules, and playbooks called in Microsoft Sentinel?**
**A: Solutions**

**Content source can either be a Solution or?**
**A: Standalone**

# Lab-02: Install Content Hub Solutions

**Context:** Your company recently deployed Microsoft Sentinel. However, no Content hub solutions have been installed yet.

**Role:** You are logged in as:

- [Microsoft Sentinel Contributor](https://learn.microsoft.com/en-us/azure/sentinel/roles#microsoft-sentinel-specific-roles)
- [Log Analytics Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/analytics#log-analytics-contributor)

<img width="954" height="329" alt="image" src="https://github.com/user-attachments/assets/935472d2-4960-4d83-bc00-08021d1889ae" />

**Lab scenario:** Following the initial deployment of Microsoft Sentinel, you are tasked with **installing a Content hub solution**.

- First, make sure **Sentinel is** **enabled for the workspace**
- Then, you will install a **Content hub solution**
- Finally, you will **review** the results

## Install a Content hub solution
- Go to **Microsoft Sentinel** and open the available workspace
- Under **Content management**, select **Content hub**

<img width="243" height="126" alt="image" src="https://github.com/user-attachments/assets/ccaf2abf-f0b2-4a93-bfd2-7c95cbcdefc4" />

- Note that currently, no standalone contents or packaged solutions are installed

<img width="833" height="190" alt="image" src="https://github.com/user-attachments/assets/4273d7fe-76bb-4335-8284-c375930191b2" />

- Search for solution: **Microsoft Entra ID**, select it and click **Install**

<img width="1601" height="682" alt="image" src="https://github.com/user-attachments/assets/b245cd82-d5c0-4a22-b0f2-65231b881ce1" />

## Review the Results

- Select the recently installed Content hub solution
- Review the Content hub **solution details** on the right pane
<img width="451" height="688" alt="image" src="https://github.com/user-attachments/assets/9c8563c0-adde-4d61-b3a6-fb033c4d1cf7" />

- Note that various **content types** are deployed by this Content hub solution:
    - 63 **Analytics Rules**
    - 1 **Data Connector**
    - 11 **Playbooks**
    - 2 **Workbooks** (dashboards)
- Click **Manage** to see the **package details** for the Content hub solution

<img width="971" height="725" alt="image" src="https://github.com/user-attachments/assets/d8a03e6a-3a60-4015-b41a-e178311bc8f6" />

- Under **Content name**, click on **Microsoft Entra ID**
- Review the data connector **status**

<img width="815" height="404" alt="image" src="https://github.com/user-attachments/assets/bd3e7ac7-c812-491e-98ba-8b43fd04cf6b" />

**What Content source type is Microsoft Entra ID?**
**A: Solution**

**What category is the Microsoft Entra ID?**
**A: Identity, Security - Automation (SOAR)**

# Connecting Data Connectors

Now that we have a **Content hub** solution installed, which comes with a data connector, the next step is to connect and configure it so that log data starts flowing into the Log Analytics workspace for Microsoft Sentinel to analyze and correlate.

To connect a data connector, the concept is to go to its **connector page** and follow the specific instructions for that connector.

<img width="1567" height="1121" alt="image" src="https://github.com/user-attachments/assets/dbebaa17-58f4-465b-a778-f993c582409a" />

- **Description:** What the connector is  

- **Data types:** **Log tables** that will be populated in the Log Analytics workspace once the connector is connected, i.e., types of log data  

- **Instructions:**
- **Prerequisites:** Permissions you need to have to connect the data
- **Configuration:** Specific config steps for the connector

**Where can you find all the details about a connector?**
**A: Connector Page**

**In order to configure the Microsoft Entra ID data connector, what permissions are required on the Log Analytics workspace?**
**A: Read and Write**

# Lab-03: Connect and Configure a Data Connector

**Context:** Your company recently deployed Microsoft Sentinel and a Content hub solution is installed. A data connector is populated; however, it has not been connected yet.

**Role:** You are logged in as:

- [Microsoft Sentinel Contributor](https://learn.microsoft.com/en-us/azure/sentinel/roles#microsoft-sentinel-specific-roles)
- [Log Analytics Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/analytics#log-analytics-contributor)

**Lab scenario:** Following the installation of the Content hub solution, you are tasked with **configuring data connectors**.

- First, you will install another Content hub solution: **Threat Intelligence**
- Then, you will connect the **data connector**
- Finally, you will **review** the ingested **Threat Intelligence** data

## Install the Threat Intelligence Content hub solution

<img width="1264" height="1052" alt="image" src="https://github.com/user-attachments/assets/f087cbd5-e83d-40ed-ac29-5f6b05817cf9" />

## Connect the Threat Intelligence Data

- After the installation is complete, select **Manage** to **review** the deployed **Threat Intelligence** (TI) connectors

<img width="399" height="722" alt="image" src="https://github.com/user-attachments/assets/7b5769fb-9815-4613-8b97-f8ab1f998741" />

- Select the **Microsoft Defender Threat Intelligence** data connector and **open the connector page**

<img width="934" height="212" alt="image" src="https://github.com/user-attachments/assets/8109a79e-523f-4ac2-bc90-31d14dbaed44" />

- For this data connector, only **workspace-scoped permissions** are needed, which you already have
- Other data connectors might have different permission requirements on various scopes depending on the integration nature of that data connector
- Click **Connect** to import **Indicators of Compromise (IOCs)** from **Microsoft Defender Threat Intelligence (MDTI)** into Microsoft Sentinel

<img width="1224" height="587" alt="image" src="https://github.com/user-attachments/assets/bd31f8cf-7d56-4138-b6fb-250fac419b45" />

**What does the Microsoft Defender Threat Intelligence (MDTI) connector import?**
**A: Indicators of compromise**

**Can threat indicators include IP addresses and domains? (Yea/Nay)**
**Yea**

