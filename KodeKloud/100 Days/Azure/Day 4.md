# Overview 

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

Create a Virtual Network (VNet) named `nautilus-vnet` in the `southcentralus` region with any `IPv4` CIDR block.

# What is an Azure Virtual Network?

An Azure Virtual Network (VNet) is an isolated network in Azure that allows you to securely connect Azure resources like VMs, databases, and services to each other, to the internet, and to on-premises networks.

# Solution

First we search for "virtual networks" in the search bar
<img width="479" height="167" alt="image" src="https://github.com/user-attachments/assets/af89e0e5-89f4-429d-964d-9e8b6410e2f3" />


Click on "+create"
<img width="1256" height="938" alt="image" src="https://github.com/user-attachments/assets/6abfdcd1-79e5-41cd-ba84-318d52901d40" />


Now we add our virtual network name `nautilus-vnet` and select the region `southcentralus`
<img width="1259" height="855" alt="image" src="https://github.com/user-attachments/assets/512a33fd-abda-4f38-88ff-c9cf3e48fe29" />

Checking the Security and IP address tab we can leave as default:
<img width="925" height="624" alt="image" src="https://github.com/user-attachments/assets/032fffd8-9ef8-4f90-a52a-bc5c67a99aa3" />

Now we can Click "Review + create" and make sure all settings are correct and we can "Create" our virtual network. Which we see the deployment will be complete within a few seconds after creating it.
<img width="972" height="376" alt="image" src="https://github.com/user-attachments/assets/e62af7a5-a9a6-48df-91fc-7b26cc0a2478" />

# Best Security Practices

- **Use subnets to segment resources** by function (web, app, database tiers)
- **Implement Network Security Groups (NSGs)** on subnets to control traffic flow
- **Use Azure Bastion** for secure RDP/SSH access instead of exposing VMs to the internet
