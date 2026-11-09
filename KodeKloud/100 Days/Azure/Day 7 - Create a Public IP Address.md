---
layout: default
title: Day 7 - Create a Public IP Address for Azure VM
description: "Working through KodeKloud's 100 Days challenges for DevOps, AWS and Azure"
---

[← Back to KodeKloud 100 Days](/kodekloud)

# Overview

For this task I had to allocate a public IP address named `nautilus-pip`.

# What is an IP address?

An IP address is a unique number given to every device on a network.

**Public IP addresses** are unique across the whole internet and can be reached from it.

**Private IP addresses** are used inside local networks, like a home, an office or an Azure VNet, and can't be reached directly from the internet.

# Solution

Search for **Public IP addresses** in the search bar and click **+ Create**.

![Searching for Public IP addresses](/KodeKloud/100%20Days/images/azure-day7/01.png)

![Public IP addresses page with + Create](/KodeKloud/100%20Days/images/azure-day7/02.png)

Most of the details can be left as default. I entered the name `nautilus-pip` and left it as IPv4, which is the standard and most widely used.

![Create public IP address with the name nautilus-pip](/KodeKloud/100%20Days/images/azure-day7/03.png)

Then **Review + create**, wait for the "Validation passed" message, and click **Create**. The deployment takes around 10 to 30 seconds.

![Deployment complete](/KodeKloud/100%20Days/images/azure-day7/04.png)

When it says "Your deployment is complete", click **Go to resource**. An IP address has been assigned (`40.76.159.240`).

![nautilus-pip with its IP address](/KodeKloud/100%20Days/images/azure-day7/05.png)

# Notes

A public IP on its own doesn't do anything until it's attached to something, like a VM's network interface or a load balancer. Every public IP is a way in from the internet, so I'd only attach one to a VM if it really needs to be reachable, and lock it down with an NSG.
