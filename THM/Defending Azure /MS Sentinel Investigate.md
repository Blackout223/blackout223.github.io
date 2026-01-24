
# Introduction

We will look into **incident investigation and management** concepts to see how we can easily manage security incidents in Microsoft Sentinel.

- Firstly, we'll introduce incident tools and features in Microsoft Sentinel
- Then, investigate sample incidents
- Finally, we'll see how we can manage incidents, hand them over, or escalate them a higher level security team

# Events/Alerts/Incidents

|              |                                                                                                                                                                                                                                                                                         |                                                                              |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Event**    | Any observable change or occurrence within a system could be routine, informational, or indicative of issues.<br><br>All technology devices create events in the form of log entries and regular status updates, which are recorded as event data in various databases and other files. | A point in time representing the state of service                            |
| **Alert**    | A notification triggered by an event designed to inform stakeholders of a situation that needs attention.                                                                                                                                                                               | Certain event(s) that meet a threshold                                       |
| **Incident** | A specific kind of negative event that disrupts normal operations or services and requires intervention.                                                                                                                                                                                | Contains correlated alerts that indicate impact, disruptive issue, or breach |
**What is a normal activity that happens at a point in time, neither good nor bad called?**
**A: Event**

**What is a negative event that disrupts normal operations and requires investigation called?**
**A: Incident**

# Incidents Overview

![Microsoft Sentinel Incidents dashboard shows 3 open incidents, 3 new incidents, and 0 active incidents. The selected incident is "Sign-ins from IPs that attempt sign-ins to disabled accounts," categorized as medium severity, unassigned owner, and new status.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1737750468291.png)  

1. Showing the number of **Open**, **New**, and **Active** incidents.
2. Show **all incidents** based on the filters applied, e.g., the last 30 days.
3. **Incident management details** for the selected incident:
- **Owner:** Unassigned or assigned owner  
- **Status:** New, Active, Closed
- **Severity:** High, Medium, Low, Informational
4.  **Evidence: Events** and **Alerts** related to the selected incident.

The right pane only summarizes the incident; more details can be found by clicking "**View full details**" for investigation purposes.

It is also important to note that the source of alerts resulting in incidents may not always be Microsoft Sentinel itself. Other security products may also contribute to incidents, as seen below. In fact, this is the most common scenario: Once connected to Microsoft Sentinel, alerts from these products will produce the majority of the incidents, such as the Microsoft Defender products suite.  

![Microsoft Defender for Cloud Apps, Microsoft Defender for Cloud, Microsoft Defender for Identity, Microsoft Entra ID Protection, Azure Information Protection.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/7161b858377c2b8866a68e7a3f4b5911.png)

**Is Microsoft Sentinel the only product contributing to incidents? (Yea/Nay)**
**A: Nay**

# Triage and Investigate - Ownership, Status, Tasks

## Incident Ownership

This individual, known as the incident owner, is responsible for overseeing the entire incident management process, encompassing investigation and providing status updates. Ownership can be transferred at any point to delegate the incident to another member of the security team for additional investigation or escalation.

As for a SOC analyst, **triaging** and **investigating** an incident usually starts by taking ownership of the incident by changing the **owner** and **status**. This could be done from the incident overview pane by:

- **Assigning** the incident **owner** to yourself.

![Incident management system interface with an incident titled "Sign-ins from IPs that attempt sign-ins to disabled accounts."](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1737745374629.png)  

- Changing the incident **status** from **New** to **Active.**

![Incident ID 3: "Sign-ins from IPs attempting access to disabled accounts." Status: unassigned, new, medium severity.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/2f2a7c38554f263be78965e3cef09b40.png)


## Incident Severity

The severity of an incident is initially determined by the rule or Microsoft security source that triggered it. Typically, the severity of an incident remains unchanged, but there's the possibility of adjusting it if you determine that the initial classification is inaccurate. Severity options range from **Informational**, **Low**, **Medium**, to **High**.

Using the **incident overview pane**, you can also **add tags** and take some initial **actions** for the incident, such as:

- **Investigate**
- **Run playbook**
- **Create automation rule**
- **Create team**

![Incident management system interface titled "Sign-ins from IPs that attempt sign-ins to disabled accounts," with Incident ID: 3. The incident is unassigned, marked as "New," and has a medium severity level.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1737745374661.png)

## Incident Details

When it's time for a more in-depth investigation, the **Incident Details** page will reveal many details for the SOC analyst. By clicking "**View full details**", we get to the page where most investigation actions can be performed.

![Incident titled "Sign-ins from IPs that attempt sign-ins to disabled accounts". The incident ID is 3, with medium severity, new status, and unassigned owner. Workspace name: law-20240213-123.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1737745374624.png)  

At the top of the page, you can find:

- **Logs:** To open the Log Analytics query window  
- **Tasks:** To see the tasks assigned for the incident  
- **Activity log:** To see any actions taken for the incident

## Incident Tasks

SOC analysts are tasked with a series of steps—**triaging**, **investigating**, or **remediating** incidents; hence, formalizing these procedures is crucial. By establishing a standardized list of tasks, your Security Operations Center (SOC) can maintain consistency across all analysts, regardless of their shifts.

!["Incident tasks" section of a task management interface. It lists three tasks: "Reset password," "Validate the alert," and "Block IPs and URLs".](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/faf8cee4fd4ada3476f0a5eabc5b6d3b.png)

This approach guarantees that every incident receives uniform treatment and adheres to Service Level Agreements (SLAs). With predefined steps set by SOC management or senior analysts (lvl 2/3), drawing from common security frameworks like NIST, past incident experience, or recommendations from security vendors, analysts can work efficiently without deliberating on the next steps or fearing missing critical actions.

**What ensures the smooth and efficient operation of security operations (SecOps)?**
**A: process standardization**

**What helps to guarantee that every incident receives uniform treatment?**
**A: Incident Tasks**

# Triage and Investigate - Timeline, Logs, Entities, Visual

## Incident Timeline
An **incident timeline** is particularly helpful for seeing the timeline of an attacker's activity. Here, you can also search the alerts by **severity** and **tactics** to narrow down the ones you want to investigate further.

![Security incident timeline and details from a cybersecurity alert system. The left side of the image displays an "Incident timeline" with a list of security events that occurred on September 26.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/0f700ebc24486d934b8fae21099fe0dc.png)

## Investigate With Logs

To investigate a specific alert without leaving the incident details page, click "**Link to LA**". This will bring up the **Log Analytics Query** page, where we can further analyze this alert.

![Microsoft Sentinel security incident management interface. The left side displays the "Incident timeline," listing multiple incidents titled "Sign-ins from IPs that attempt sign-ins..." with timestamps, all marked as "Medium" severity, and detected by Microsoft.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6601e243753b8d484668851e/room-content/6601e243753b8d484668851e-1736972480721.png)  

Based on the **rule logic**, certain **entities** could be of interest here, such as IP addresses, accounts, etc.  

![Log analysis dashboard with a query and its results. The query is designed to identify login attempts to disabled accounts.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/fbfada009541178e65788ef4f10a34fd.png)

## Incident Entities

When Microsoft Sentinel generates alerts, they contain data items that Sentinel can recognize and classify into categories as **entities**. Here are some entity types recognized by Microsoft Sentinel:

- Account
- Host
- IP address
- URL
- Malware
- Process

Knowing the IP address entity involved in the incident, incident enhancement playbooks can be applied to provide more context for this IP. For instance, these automation playbooks can call APIs such as **[IP-API](https://ip-api.com/)** to get **geolocation** details for the IP.

![Microsoft Sentinel, showing details of a security incident involving the IP address 175.45.176.99.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/8ef49bd8037dd41f3772f9cd1909131d.png)


## Visual Investigation

Incidents can also be investigated visually. If you prefer a visual, graphical representation of alerts, entities, and the connections between them in your investigation, you can do so by clicking the **Investigate** button on the Incident Details page.  

![Section of a user interface related to cybersecurity tactics and techniques.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/ae56daccdb6fa607f6ce7d725e0ecbcd.png)

![Security incident report titled "Sign-ins from IPs that attempt sign-ins to disabled accounts" with a medium severity level and a new status.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/fe00834196b31581d15de79ae59b89b9.png)  

You can further deepen your investigation by hovering over each entity. This will show a list of additional questions per entity type. These are called **exploration queries.**

![Security interface showcasing a diagram of connections related to a specific IP address, 175.45.176.99. The central node shows the IP address, and multiple connected nodes representing alerts and sign-in events from various IPs.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efbaebdaaea011c857b438d/room-content/9970d90425acc2830418242c74db015f.png)

**Which link can you click to see the specific logs of an alert without leaving the context of the incident details page?**
**A: Link to LA**

**During graphical investigation, which queries can be utilized to deepen the investigation?**
**A: Exploration**

# Incident Closure - True/Benign/False Positives

# Closing Incidents

When an incident is generated in Microsoft Sentinel, it is automatically labelled as "**New**". As you access and address these incidents, manually update their status to accurately represent their current state. For incidents undergoing investigation, you designate the status as "**Active**". Once an incident is completely resolved, mark it as "**Closed**".

Once an incident is closed, select the classification:

|   |   |
|---|---|
|**Classification**|**Description**|
|True Positive|Suspicious activity|
|Benign Positive|Suspicious but expected|
|False Positive|Incorrect alert logic|
|False Positive|Inaccurate data|
|Undetermined|---|
**True Positive -** **Suspicious Activity**

- The root cause was an actual threat.  
    

**Benign Positive -** **Suspicious but Expected**

- The action detected is **real** but **not malicious**. For instance:  
    - Penetration tests
    - Elevating privileges to deploy system updates  
        

**False Positive - Inaccurate Data**  

- **Data** contained in the incident details is inaccurate
    - Wrong user account, IP, etc.
    - Analytics rule may need tuning  
        

**False Positive - Incorrect Alert Logic**  

Microsoft Sentinel **Analytics rules** alert you when suspicious activity arises within your network. Recognizing that no analytics rule is flawless, it's inevitable that you'll encounter false positives requiring attention.  
  
Some **false positives** can occur due to some common activities, such as the following:

- Routine activities performed by certain users, often service principals, exhibit patterns that appear suspicious
- Purposeful security scanning originating from recognized IP addresses being flagged as malicious
- A rule designed to exclude private IP addresses may inadvertently include some internal IP addresses that aren't private

On a high level, these false positives can be eliminated with the following methods:

- **Automation rules**
    - By creating exceptions without modifying Analytics rules
- **Scheduled Analytics rules modifications**
    - By allowing more detailed and permanent exceptions
- **Undetermined**
    - You are unable to determine the cause of the incident
    - You might get a similar incident in the future; however, for the current, you don't have enough info for a conclusion

**You investigate an incident and conclude that it is due to red team activities. What is your closure classification?**
**A: benign positive**

**How do you eliminate a false positive without modifying the analytics rule?**
**A: automation rule**

**What do you classify an actual threat as?**
**A: True Positive**

# Lab-05: Investigate Incidents

**Context:** In your company's Microsoft Sentinel environment, previously enabled Analytics rules started to generate incidents.

**Role:** You are logged in as:

- [Microsoft Sentinel Contributor](https://learn.microsoft.com/en-us/azure/sentinel/roles#microsoft-sentinel-specific-roles)
- [Log Analytics Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/analytics#log-analytics-contributor)

**Lab scenario:** As a **SOC LVL 1 Analyst**, you need to start **triaging and investigating the incidents**. You will:

- First, review open incidents and **take ownership**
- Then, **investigate** the **incident**
- Finally, **escalate** the incident to SOC LVL 2 for **in-depth investigation** and **threat hunting**


**What is the MITRE Tactics classification for Solorigate incidents?**
![[Pasted image 20250416165024.png]]
**A: Command and Control**

**Investigate the incident "Sign-ins from IPs that attempt sign-ins to disabled accounts". What is the IP entity involved in this incident?**
![[Pasted image 20250416171544.png]]
**A: 175.45.176.99**

**Check out this IP's geolocation. What is the city?**
**A: Pyeongyang**

**Now, dive back into the alert logs for this incident. Which disabled account was targeted by attackers?**
![[Pasted image 20250416171843.png]]
**A: johns@m365x816222.onmicrosoft.com

**How many login attempts were there for this disabled account?**
**A: 4**

