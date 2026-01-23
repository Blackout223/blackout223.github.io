---
layout: default
title: Azure Day 5 - Create Virtual Network (IPv4) in Azure
---

# Overview

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the Azure cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Networks (VNets) as the initial step, as they will be provisioning various services under different VNets.

Create a Virtual Network (VNet) named `devops-vnet` in the `westus` region with `192.168.0.0/24` IPv4 CIDR.

# What is a Virtual network?

An Azure Virtual Network (VNet) is an isolated network in Azure that allows you to securely connect Azure resources like VMs, databases, and services to each other, to the internet, and to on-premises networks.

# Solution

First, search for Virtual Networks in the search bar
<img width="501" height="173" alt="image" src="https://github.com/user-attachments/assets/c1f06c19-a072-4794-b7b0-b37753e5e44d" />


On the Virtual networks page, click the **"+ Create"** button to begin VNet creation.
<img width="1266" height="993" alt="image" src="https://github.com/user-attachments/assets/6f6c391a-050a-4733-9529-3f4975f43618" />

Add the virtual network name:
`devops-vnet`

Swap the region to:
`West US`
<img width="1192" height="798" alt="image" src="https://github.com/user-attachments/assets/bdfb3ea2-bb42-4f95-a388-ee3dafa855b2" />


After verifying all fields were correct, head to **IP Addresses** tab:
<img width="1233" height="672" alt="image" src="https://github.com/user-attachments/assets/09a4dc44-a3f6-44ef-ac46-1a7c560027ab" />


The **IP Addresses** tab displayed a default address space that needed to be replaced with our specific requirements. Which can be removed.

Now the IP address we need has to be added. Which can be done by selecting **Add IPv4 address space**
<img width="925" height="504" alt="image" src="https://github.com/user-attachments/assets/cd03f005-ac5d-48f5-99b1-d48aaf26bf5f" />


Enter the IP address range, that is going to be used:
`192.168.0.0/24`
<img width="1252" height="639" alt="image" src="https://github.com/user-attachments/assets/ffc1d868-5c39-4f14-a176-4750044b859f" />

Now we can "Review+Create" - Which we get a validation passed message and can click create again:
<img width="1252" height="1262" alt="image" src="https://github.com/user-attachments/assets/8d03e585-29bd-4ef4-92ef-94af6dfc5490" />


It will take up a few seconds for it to be deployed, which we can see the deployment has been complete:
<img width="968" height="424" alt="image" src="https://github.com/user-attachments/assets/8e14547f-c85f-4860-9494-60003ff037bc" />


