
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

![The Azure portal shows the "Job function roles" tab, filtered for roles related to "Microsoft Sentinel," including "Microsoft Sentinel Automation Contributor," "Microsoft Sentinel Contributor," "Microsoft Sentinel Playbook Operator," "Microsoft Sentinel Reader," and "Microsoft Sentinel Responder."](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/a1db6e21c255adb58a4b17683e301759.png)

**Lab scenario:** Following the initial deployment of Microsoft Sentinel, you are tasked with **installing a Content hub solution**.

- First, make sure **Sentinel is** **enabled for the workspace**
- Then, you will install a **Content hub solution**
- Finally, you will **review** the results

## Install a Content hub solution
- Go to **Microsoft Sentinel** and open the available workspace
- Under **Content management**, select **Content hub**

![Content management menu with three options: Content hub, Repositories (Preview), and Community.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/25077fc416f36e712a978f9cdb858c6d.png)

- Note that currently, no standalone contents or packaged solutions are installed

![Microsoft Sentinel Content Hub. The statistics at the top indicate 342 Solutions and 273 Standalone contents available, with 0 Installed and 0 Updates.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/eec02355464674cbb50459df9e65a08b.png)

- Search for solution: **Microsoft Entra ID**, select it and click **Install**

![User interface for selecting and installing software in Microsoft Sentinel. "Microsoft Entra ID" is highlighted as "FEATURED." The description includes provider, support, version, and a note to read release notes before installation.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/7ddaa2584fc741700a7448dd4da8dca6.png)

## Review the Results

- Select the recently installed Content hub solution
- Review the Content hub **solution details** on the right pane
![Microsoft Entra ID solution for Microsoft Sentinel.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/c9570e7892a8906e5202ef928cd10816.png)

- Note that various **content types** are deployed by this Content hub solution:
    - 63 **Analytics Rules**
    - 1 **Data Connector**
    - 11 **Playbooks**
    - 2 **Workbooks** (dashboards)
- Click **Manage** to see the **package details** for the Content hub solution

![A Microsoft Sentinel Content hub interface displaying content items for Microsoft Entra ID. It shows 77 installed content items and 75 configuration-needed.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/6c17e15ef8dd539eb08e61da0ed7c7c1.png)

- Under **Content name**, click on **Microsoft Entra ID**
- Review the data connector **status**

![A "Data connectors" interface from a software application. It displays a summary with 1 connector available and 0 connected.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/8b3b304e4a3fe3ceac0c8c9641178e63.png)

**What Content source type is Microsoft Entra ID?**
**A: Solution**

**What category is the Microsoft Entra ID?**
**A: Identity, Security - Automation (SOAR)**

# Connecting Data Connectors

Now that we have a **Content hub** solution installed, which comes with a data connector, the next step is to connect and configure it so that log data starts flowing into the Log Analytics workspace for Microsoft Sentinel to analyze and correlate.

To connect a data connector, the concept is to go to its **connector page** and follow the specific instructions for that connector.

![Microsoft Sentinel Data connectors page with the "Microsoft Entra ID" connector selected.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1736639419064.png)

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

![[Pasted image 20250412202410.png]]

## Connect the Threat Intelligence Data

- After the installation is complete, select **Manage** to **review** the deployed **Threat Intelligence** (TI) connectors

![Threat intelligence solution for Microsoft Sentinel with description.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1726532526297.png)

- Select the **Microsoft Defender Threat Intelligence** data connector and **open the connector page**

![Different threat intelligence data connectors. The columns are labeled "Content name," "Created content," "Content type," and "Version."](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1726532526666.png)

- For this data connector, only **workspace-scoped permissions** are needed, which you already have
- Other data connectors might have different permission requirements on various scopes depending on the integration nature of that data connector
- Click **Connect** to import **Indicators of Compromise (IOCs)** from **Microsoft Defender Threat Intelligence (MDTI)** into Microsoft Sentinel

![Microsoft Defender Threat Intelligence (Preview) configuration screen in Microsoft Sentinel.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/9b3951acfb8d833eb3965868ce980270.png)

**What does the Microsoft Defender Threat Intelligence (MDTI) connector import?**
**A: Indicators of compromise**

**Can threat indicators include IP addresses and domains? (Yea/Nay)**
**Yea**

