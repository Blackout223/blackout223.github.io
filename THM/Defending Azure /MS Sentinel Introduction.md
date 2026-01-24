# Microsoft Security Operations Analyst

A **Security Operations Center (SOC)** is a centralized security unit with team(s) responsible for protecting the organization against security threats.

The main goal is to reduce organizational risk. The mission statement would include the following points:  

- **Remediate active attacks** in the environment.
- **Advise on improvements** to threat protection practices.
- **Refer violations** of organizational policies to appropriate stakeholders.

## Responsibilities

|   |   |
|---|---|
|**Monitoring**|[SOC Level 1 Analyst](https://tryhackme.com/r/resources/blog/become-level-1-soc-analyst)|
|**Triage**|[SOC Level 1 Analyst](https://tryhackme.com/r/resources/blog/become-level-1-soc-analyst)|
|**Incident Response**|[SOC Level 2 Analyst](https://tryhackme.com/r/resources/blog/become-level-2-soc-analyst)|
|**Threat Hunting**|[SOC Level 2 Analyst](https://tryhackme.com/r/resources/blog/become-level-2-soc-analyst)|
|**Advanced Threat Hunting**|SOC Level 3 Analyst|
|**Threat Intelligence (TI) Analysis**|SOC Level 3 Analyst|
|**Vulnerability Management**|SOC Level 3 Analyst|
**What security unit is responsible for protecting the organization against security threats?**
**A: Security Operations Center**

**Generally, which level of SOC Analyst is responsible for responding to incidents?**
**A: SOC Level 2 Analyst**

**Besides monitoring, what else do SOC Level 1 Analysts spend the majority of their time with?**
**A: Triage**

# Introduction to Microsoft Sentinel

## What is SIEM

SIEM stands for **Security Information and Event Management**. It is a comprehensive approach to security management that combines **Security Information Management** (SIM) and **Security Event Management** (SEM) into a single solution. The primary purpose of SIEM is to provide a _holistic view_ of an organization's information security by collecting and analyzing log data from various sources across its IT infrastructure.

## What is SOAR

SOAR stands for **Security Orchestration, Automation, and Response**. It is a set of technologies and practices designed to improve the efficiency and effectiveness of an organization's cyber security operations. SOAR platforms integrate security orchestration and automation to streamline and accelerate incident response processes.


## What is Microsoft Sentinel

 Microsoft Sentinel's own definition becomes a combination of the two. It is essentially a scalable, cloud-native solution that provides the following:

- Security Information and Event Management (SIEM) functionality by:
    - Collecting and querying logs
    - Doing **correlation** or **anomaly detection**
    - Creating **alerts** and **incidents** based on findings
- Security Orchestration, Automation, and Response (SOAR) functionality by:
    - Defining playbooks
    - Automating threat responses  
    

Microsoft Sentinel also delivers security analytics and threat intelligence across the organization. It's a one-stop-shop and bird's-eye view solution for:

- Attack detection
- Threat visibility
- Proactive hunting
- Threat response

  

![A circular image showcasing Microsoft Sentinel, a cloud-native SIEM+SOAR solution. At the center is a cloud icon with a shield labeled "Microsoft Sentinel." Surrounding it are four sections: "Collect security data" (computer screen), "Detect threats" (security camera), "Investigate incidents with AI" (laptop), and "Automate response" (gears).](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1734522464655.png)  

Microsoft Sentinel performs the above actions and enables security operations by means of 4 main pillars:

- **Collect**
- **Detect**
- **Investigate**
- **Respond**

**Microsoft Sentinel is a combination of two security concepts, namely SIEM and which other one?**
**A: SOAR**

**Creating security alerts and incidents is part of which security concept?**
**A: SIEM**

**By means of how many pillars does Microsoft Sentinel help us to perform security operations?**
**A: 4**

# How Microsoft Sentinel Works

## Phase 1: Collect

- **Data connectors:** The first step is to **ingest data** into Microsoft Sentinel. This is exactly what data connectors are for. There are 100+ connectors to cover all various data sources and scenarios.

- **Log retention:** Once the data has been ingested into Microsoft Sentinel, it must be stored for further **correlation** and **analysis**. This log storage mechanism is called **Log Analytics workspaces**. Data stored in these workspaces can be queried to gain further insights using Kusto Query Language (KQL).

## Phase 2: Detect

- **Workbooks:** Workbooks are essentially **dashboards** in Microsoft Sentinel used to visualize data. There are many built-in workbooks, and custom ones can also be created by utilizing KQL.

- **Analytics rules:** Analytics rules provide proactive analytics so that SOC teams get notified when suspicious things happen. The output of running Analytics rules are security alerts and incidents.  

- **Threat hunting:** Microsoft Sentinel has over 200 built-in threat-hunting queries to support that needle-in-a-haystack job.

## Phase 3: Investigate

- **Incidents and investigations:** Once Analytics rules detect suspicious activities, i.e., once an alert is triggered, security incidents are created for SOC analysts to triage and **investigate**. Typical incident management activities include:
    - **Changing** the incident **status**
    - **Assigning** to other analysts for further investigation
    - **Mapping** entities to the investigation
    - Investigating the incident **timeline**
    - Deep-dive into investigation details using **investigation maps**  
    - Recording **investigation comments**

## Phase 4: Respond

 Alert fatigue occurs occurs when cyber security professionals are inundated with a high volume of security alerts, which leads to a diminished ability for SOC teams to react effectively to and investigate real threats.
- **Automation via playbooks:** One of the main challenges of a SOC team is alert fatigue. To overcome alert fatigue, automation in security operations is a must. This is done by **automated workflows**, also known as **playbooks**, in response to events. By doing so, automated responses can be provided for:
    - Incident management
    - Enrichment
    - Investigation
    - Remediation

**What is used to ingest data into Sentinel?**
**A: Data Connectors**

**Where are the ingested logs stored for further correlation and analysis?**
**A: log analytics workspaces**

**Workbooks are essentially `_______` used for visualization**
**A: dashboards**

**When SOC teams are flooded with security alerts and incidents, this is called?**
**A: alert fatigue**

**In Microsoft Sentinel, automation is done via automated workflows, known as?**
**A: playbooks**

**The output of running Analytics rules includes security alerts and?**
**A: Incidents**

# When To Use Microsoft Sentinel

 When there is a necessity to monitor cloud and on-premises infrastructures for security.

Opt for Microsoft Sentinel if the organization aims to:

- Gather event data from diverse data sources
- Execute security operations on the collected data to pinpoint suspicious activities

Also, some of Microsoft Sentinel's additional features are:

- **Cloud-native SIEM** - No need for server provisioning, facilitating seamless scalability
- Easy integration with Azure services and its extensive range of connectors
- Centralized monitoring
- Automated incident response
- Real-time advanced threat detection  
- Leveraging Microsoft's **Research and Machine Learning** capabilities
- Support for hybrid **cloud and on-premises** environments

If the organization has requirements such as:

- Support for data from multiple cloud environments
- Features and functionality necessary for a Security Operations Center (SOC) without excessive administrative overhead

 Could also use both in conjunction by ingesting Defender for Cloud alerts into Microsoft Sentinel, which would enhance the overall security framework.

**Organizations use Microsoft Sentinel mainly because they need to `_______` their cloud infrastructure.**
**A: monitor**

**With Microsoft Sentinel, there is no need for server provisioning. This means it is?**
**A: cloud-native**

