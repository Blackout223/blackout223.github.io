---
layout: default
title: Azure Day 6 - Create a Subnet in Azure Virtual Network
---

# Overview

For this task, create a Virtual Network (VNet) named `nautilus-vnet` and one subnet named `nautilus-subnet` within the VNet in the `southcentralus` region. Make sure the `IPv4 address range` is `10.0.0.0/16`.

# What is a subnet?

A subnet (subnetwork) is a logical subdivision of an IP network created by using a subnet mask to partition the network's IP address space into smaller, manageable segments. It works by borrowing bits from the host portion of an IP address to create additional network identifiers, allowing routers to determine which devices are on the same local network segment versus which require routing to reach, thereby improving network performance, security, and address allocation efficiency.

# Solution

First, search for Virtual Networks in the search bar
<img width="501" height="173" alt="image" src="https://github.com/user-attachments/assets/4bb17db1-6591-4318-9400-b4e7b7b85f34" />

On the Virtual networks page, click the **"+ Create"** button to begin VNet creation.
<img width="1266" height="993" alt="image" src="https://github.com/user-attachments/assets/3078e8c5-80d2-4ffe-99ac-b2497ddee48c" />


Add the virtual network name:
`nautilus-vnet`

Swap the region to:
`South Central US`

<img width="1164" height="697" alt="image" src="https://github.com/user-attachments/assets/a36ba37f-1531-470d-b052-3373d0c75cb3" />

After all fields are filled in correctly, head to **IP Addresses** tab, which the IP we need to use is already the default(`10.0.0.0/16`). To create our subnet, we need to click the pencil icon to add it:
<img width="865" height="508" alt="image" src="https://github.com/user-attachments/assets/24157498-94d6-43c5-8720-68ccf6ad45e9" />

This will open up a sidebar - Which we can fill out the name to `nautilus-subnet`:
<img width="591" height="1244" alt="image" src="https://github.com/user-attachments/assets/7f61f3fc-fad0-46a0-b7fb-240a593d05ce" />

When it's saved, it will show up with the name, IP address range and size (How many IP addresses there are).
<img width="850" height="270" alt="image" src="https://github.com/user-attachments/assets/c4f166f3-cedf-47e6-a41c-dc7082c4f127" />

This can now be Reviewed and Created

Validation has passed, now we can fully create the virtual network. After around 20-30 seconds, the deployment will be completed:
<img width="969" height="382" alt="image" src="https://github.com/user-attachments/assets/ce9da2de-6946-42ec-b74b-4dd33164c0ba" />
