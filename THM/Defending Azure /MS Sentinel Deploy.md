# Getting Ready for Deployment

## Azure Service vs. Azure Resource

The difference between an Azure Resource and an Azure Service:

|                                                              |                                                      |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| **Azure Resource**                                           | **Azure Service**                                    |
| Something you do create in Azure                             | Something you do NOT create in Azure                 |
| An instance                                                  | It is always there, provided for you                 |
| Consumes either computing, networking, or storage            | Gets added to a resource OR enabled for a resource   |
| -                                                            | To provide additional functionality to that resource |
| E.g. Virtual Machine, Azure Storage, Log Analytics workspace | E.g. Microsoft Sentinel, Azure Policy, Entra ID      |

## Microsoft Sentinel Architecture

The core component of MS Sentinel architecture is Log Analytics workspaces (LAWs). Essentially, a LAW is an Azure **_resource_** where the logs are stored

When it comes to implementing Microsoft Sentinel, there are mainly three options:

- **Single Tenant - Single Log Analytics workspace**  
    - **Central repo** for all logs across all resources
        - Central pane of glass
        - Consolidated logs  
        
    - **Potential concerns:** Logs will travel across Azure regions. As a result of this:  
        - **Bandwidth costs** due to logs travelling across regions  
        
        - **Data governance issues** due to data residency requirements  

- **Single Tenant - Multiple** **Log Analytics workspaces** **(Regional)  
    **
    - In order to address the logs travelling across regions, regional LAWs can be created in different regions.
    - Although this might address bandwidth costs and data governance concerns, on the other hand, it will bring other concerns, such as:
        - No central pane of glass
        - Granular access control
        - Granular retention settings  
- **Multi-Tenant**
    - If LAWs are not home tenants, then a multi-tenant architecture will be needed.
    - An example of this architecture would be cloud service providers.
        - I.e., when a security organization needs to monitor other organizations' Microsoft Sentinel deployments.  

To recap, on a high level, the following considerations will shape your Microsoft Sentinel architecture and deployment options:

- Tenancy  
- Compliance
- Region
- Access

**Is Microsoft Sentinel a resource or a service?**
**A: Service**

**What is a potential concern due to logs travelling across the Azure regions?**
**A: Bandwidth Costs**

# Log Analytics Workspace

## What is a Log Analytics Workspace

 Log Analytics workspaces are crucial in **collecting**, **storing**, and **analyzing log data** from various sources to provide security insights and threat detection capabilities.

Here's a breakdown of what Log Analytics workspaces are in Microsoft Sentinel:

- **Data Collection**: Log Analytics workspaces aggregate log data from security devices, servers, applications, and cloud services, including Azure services (e.g., Microsoft Defender for Cloud, Microsoft Entra ID, Azure Monitor) and non-Azure sources via agents or connectors.

- **Data Storage**: Log Analytics workspaces store log data in a structured format using the Log Analytics data model, enabling efficient querying and analysis of large data volumes. Data retention is based on configured settings, supporting historical analysis and compliance.

-  **Querying and analysis:** Users can leverage the Log Analytics query language, **Kusto Query Language (KQL),** to analyze the log data stored in the workspace. KQL provides a powerful and flexible way to query and manipulate data, enabling security analysts to identify patterns, anomalies, and potential threats within their environment.

- **Visualization and Reporting:** Log Analytics workspaces offer built-in visualization tools and dashboards to help users visualize query results and key metrics. These visualizations can aid in understanding the environment's security posture, detecting trends, and identifying areas that require attention.

- **Integration with Microsoft Sentinel:** Log Analytics workspaces integrate with Microsoft Sentinel, a cloud-native SIEM solution. Sentinel uses workspace data for advanced threat detection, hunting, and investigation, leveraging machine learning, behavioral analytics, and threat intelligence for real-time threat response.

**What does a Log Analytics workspace, where log data from different sources is aggregated, essentially serve as?**
**A: Centralized repository**

**How can we also refer to a Log Analytics workspace once Microsoft Sentinel is enabled on it?**
**A: Microsoft Sentinel Workspace**

# How to Create a LAW

To enable Microsoft Sentinel, **Microsoft Sentinel Contributor** permissions are required at the resource group level where the Microsoft Sentinel workspace resides.

1. In the Azure portal, search for **Sentinel** at the search bar and select **Microsoft Sentinel**.  
    
<img width="367" height="167" alt="image" src="https://github.com/user-attachments/assets/94ca2ca1-3be5-4dbe-8abb-2ff835025f64" />

2. Since there is no previously created Log Analytics workspace with Sentinel enabled, select **Create Microsoft Sentinel**.  
    
<img width="1035" height="771" alt="image" src="https://github.com/user-attachments/assets/93ae097d-7260-41e5-afe2-356182f21acb" />

3. Creating a Microsoft Sentinel instance enables or adds the service to a LAW. First, we need to create a LAW instance by clicking **Create a new workspace**.  
    
<img width="1035" height="771" alt="image" src="https://github.com/user-attachments/assets/faf1f47d-fecf-4c32-98e1-b77f20d75f96" />

4. Provide the below details and click **Create + Review**, followed by clicking **Create**.
    - **Subscription:** Select the subscription available in the drop-down
    - **Resource Group:** Select the resource group available in the drop-down
    - **Name**: Name of LAW instance  
    
    - **Region**: Azure region where the **ingested log data will reside**. This setting is significant as the organization might have certain data residency requirements. It is important to note that once created, the **region of a LAW cannot be changed**  
    
<img width="1035" height="771" alt="image" src="https://github.com/user-attachments/assets/3aeecc06-9a2d-42d8-bd88-d79c0f9966fc" />

## Adding Microsoft Sentinel to a Log Analytics Workspace

1. Now, we can add Microsoft Sentinel to the previously created LAW by clicking the **Add** button at the bottom.  

<img width="780" height="927" alt="image" src="https://github.com/user-attachments/assets/a7603e95-1a32-4742-bf5c-127c0c6b6d3b" />

2. After a while, Microsoft Sentinel is ready to go.  

<img width="1320" height="982" alt="image" src="https://github.com/user-attachments/assets/21d4e014-4f35-471b-b876-c7457ecea1cf" />

**What permissions do you need to enable Microsoft Sentinel?**
**A: Microsoft Sentinel Contributor**

**Do you need to have a Log Analytics workspace created before you can enable/onboard Microsoft Sentinel? (yea/nay)**
**A: Yea**

**While creating a LAW instance, which setting defines where the log data will reside?**
**A: Region**

# Lab-01: Deploy Microsoft Sentinel

Created a LAW and added sentinel

**Is every Log Analytics workspace a Microsoft Sentinel workspace? (Yea/Nay)**
**A: Nay**

**What do you need to create before enabling or onboarding Microsoft Sentinel?**
**A: Log analytics workspace**

# MS Sentinel Roles and Permissions

- **Microsoft [Sentinel Reader](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-reader)**
    - View data, incidents, workbooks, and other Microsoft Sentinel resources
- **Microsoft [Sentinel Responder](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-responder)**  (in addition to the above **Sentinel Reader**)
    - Manage incidents  
        
- **Microsoft [Sentinel Contributor](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-contributor)** (in addition to the above **Sentinel Responder**)
    - Install/Update solutions from the Content hub.
    - Create and edit workbooks, analytics rules, and other Microsoft Sentinel resources
- **Microsoft [Sentinel Playbook Operator](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-playbook-operator)**
    - List, view, and manually run playbooks
- **Microsoft [Sentinel Automation Contributor](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-automation-contributor)** (not for user accounts)  
    - Allows Microsoft Sentinel to add playbooks to automation rules

|   |   |
|---|---|
| **Stakeholders, SOC managers,** and more | [**Microsoft Sentinel Reader**](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-reader)
| **Security analysts, incident responders** (usually SOC Level-1 Analysts) | [**Microsoft Sentinel Responder**](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-responder)
| **Security engineers, Fusion Analytics team members** (usually SOC Level-2 Analysts)<br><br>Install and manage **Solutions** using **Content Hub**<br><br>Create and delete workbooks | [**Microsoft Sentinel Contributor**](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-contributor)
| **Automation team members** (usually SOC Level-1 Analysts)  <br>  <br>Automate responses to threats with **playbooks** | [**Microsoft Sentinel Playbook Operator**](https://learn.microsoft.com/en-ca/azure/role-based-access-control/built-in-roles#microsoft-sentinel-playbook-operator)
As a best practice, try to assign these roles to the Resource Group where Microsoft Sentinel workspace resides, as there might be other resources in that resource group which support the functionality of Microsoft Sentinel.

**You are an incident responder who needs to manage incidents, such as assigning and dismissing them. Which role do you need?**

**A: Microsoft Sentinel Responder**

**You are a security engineer, and you are tasked with enabling pre-packaged Solutions from Content Hub. Which role do you need?**

**A: Microsoft Sentinel Contributor**

**You are the CISO of the organization. You need visibility into Sentinel data, but you don't directly manage the Sentinel environment. Which role do you need?**

**A: Microsoft Sentinel Reader**

# MS Sentinel Settings

 While working with Sentinel, there will be two sets of settings that you need to maintain.

- **Microsoft Sentinel settings**
- **Log Analytics workspace settings**  

That's why some functionality changes in Microsoft Sentinel require you to modify underlying Log Analytics workspace settings.

## Data Retention

An important feature of sentinel **Data Retention**. It is also important to note that Microsoft Sentinel is billed based on the **volume of data ingested** for analysis.

The more data you have—or keep in the Log Analytics workspace—the more relevant and accurate threat analysis you can perform.

Microsoft Sentinel settings can be found at `Microsoft Sentinel > Select the workspace > Configuration > Settings`.

<img width="269" height="434" alt="image" src="https://github.com/user-attachments/assets/bdc26f16-e0e0-47d7-9d3f-5f0e3876a512" />

To modify **Data Retention**, we need to go to **Workspace settings** since data is stored in the underlying Log Analytics workspace for Microsoft Sentinel.

<img width="418" height="601" alt="image" src="https://github.com/user-attachments/assets/c6c7f642-31e8-4179-b2c9-c579ddb6fcf5" />

**Data Retention** settings are tucked into **Usage and estimated costs** under **Settings**.

<img width="911" height="458" alt="image" src="https://github.com/user-attachments/assets/bf7abb9b-26a8-4399-8860-528260a6d1cd" />

Default data retention is 31 days - Might need to increase it due to governance

Additional charges if longer retention is needed as sentinel billing is bases on log data storage

It is important to monitor the organization's log usage details regularly.

<img width="417" height="394" alt="image" src="https://github.com/user-attachments/assets/f0664508-2f44-402a-be45-0fc14b9bb917" />

**How many sets of settings are there for Microsoft Sentinel?**

**A: 2**

**Can Data Retention settings be found under Microsoft Sentinel workspace settings? (Yea/Nay)**

**A: Yea**

**Under which subcategory of settings are the Data Retention settings tucked in?**

**A: Usage and estimated costs**

