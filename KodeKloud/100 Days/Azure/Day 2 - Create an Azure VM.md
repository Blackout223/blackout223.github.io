---
layout: default
title: Day 2 - Create an Azure VM
---

# Overview

For this task I had to create a VM with these requirements:

1. Use the existing resource group
2. Name `devops-vm` in the `West US` region
3. `Ubuntu 22.04 LTS` image
4. Size `Standard_B1s`
5. Default Network Security Group (NSG) allowing inbound SSH on port 22
6. A 30 GB `Standard HDD` data disk
7. Everything else left as default

Then check I could SSH into it.

# Solution

I searched for **Virtual machines** at the top of the portal.

![Searching for virtual machines](/KodeKloud/100%20Days/images/azure-day2/01.png)

Then picked **Azure virtual machine** from the Create drop-down.

![Create drop-down](/KodeKloud/100%20Days/images/azure-day2/02.png)

I filled in the Basics tab with the name, region, image and size from the task, and allowed inbound SSH.

![Basics tab](/KodeKloud/100%20Days/images/azure-day2/03.png)

![Basics tab continued](/KodeKloud/100%20Days/images/azure-day2/04.png)

On the **Disks** tab I changed the OS disk type to Standard HDD, then created and attached a new 30 GB disk.

![Disks tab](/KodeKloud/100%20Days/images/azure-day2/05.png)

Then **Review + create**. Validation passed so I created the VM.

![Validation passed](/KodeKloud/100%20Days/images/azure-day2/06.png)

It took a minute or two to deploy.

![VM deployed](/KodeKloud/100%20Days/images/azure-day2/07.png)

Then I tested SSH with the key I downloaded while creating the VM:

```sh
ssh -i <keyname> azureuser@<public-ip>
```

It worked. The VM name in this screenshot is different because I had to recreate the VM after an issue with the lab.

![SSH into the VM](/KodeKloud/100%20Days/images/azure-day2/08.png)

# Notes

The default NSG rule allows SSH from any IP. That's fine for a short lab but on a real VM I'd:

- limit port 22 to a known IP range, or use Azure Bastion so the VM doesn't need a public SSH port at all
- keep key-only login and leave password authentication off
- turn on disk encryption and backups
