---
layout: default
title: Azure Day 6 - Create a Subnet in Azure Virtual Network
---

# Overview

For this task, create a Virtual Network (VNet) named `nautilus-vnet` and one subnet named `nautilus-subnet` within the VNet in the `southcentralus` region. Make sure the `IPv4 address range` is `10.0.0.0/16`.

# What is a subnet?

A subnet is a smaller range carved out of the VNet's address space. You put resources in subnets so you can group them and apply different rules to each group. For example, web servers in one subnet and databases in another, with an NSG controlling what can reach the databases.

# Solution

First, search for Virtual Networks in the search bar
<img width="501" height="173" alt="image" src="https://github.com/user-attachments/assets/4bb17db1-6591-4318-9400-b4e7b7b85f34" />

On the Virtual networks page, click the **"+ Create"** button to begin VNet creation.
<img width="1266" height="993" alt="image" src="https://github.com/user-attachments/assets/3078e8c5-80d2-4ffe-99ac-b2497ddee48c" />


Add the virtual network name:
`nautilus-vnet`

Swap the region to:
`South Central US`

<img alt="Create virtual network with the name nautilus-vnet" src="/KodeKloud/100%20Days/images/azure-day6/03.png" />

After all fields are filled in correctly, head to **IP Addresses** tab, which the IP we need to use is already the default(`10.0.0.0/16`). To create our subnet, we need to click the pencil icon to add it:
<img alt="Editing the default subnet" src="/KodeKloud/100%20Days/images/azure-day6/04.png" />

This will open up a sidebar - Which we can fill out the name to `nautilus-subnet`:
<img alt="Renaming the subnet to nautilus-subnet" src="/KodeKloud/100%20Days/images/azure-day6/05.png" />

When it's saved, it will show up with the name, IP address range and size (How many IP addresses there are).
<img alt="nautilus-subnet in the address space" src="/KodeKloud/100%20Days/images/azure-day6/06.png" />

This can now be Reviewed and Created

Validation has passed, now we can fully create the virtual network. After around 20-30 seconds, the deployment will be completed:
<img alt="Deployment complete" src="/KodeKloud/100%20Days/images/azure-day6/07.png" />
