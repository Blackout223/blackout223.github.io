---
layout: default
title: Day 8 - Attach Managed Disk to Azure VM
description: "Working through KodeKloud's 100 Days challenges for DevOps, AWS and Azure"
---

[← Back to KodeKloud 100 Days](/kodekloud)

# Overview

A VM named `nautilus-vm` and a managed disk named `nautilus-disk` already exist in the `southcentralus` region. The task was to attach `nautilus-disk` to `nautilus-vm` as a data disk, and make sure it's attached.

# What is a virtual disk?

A virtual disk works like a physical hard drive or SSD for a VM. The OS disk holds the operating system, and data disks hold applications and data. Keeping data on a separate disk means it can be detached and moved to another VM, or kept when the VM is deleted.

# Solution

Search for **Virtual machines** in the portal and select `nautilus-vm`.

![Searching for virtual machines](/KodeKloud/100%20Days/images/azure-day8/01.png)

![nautilus-vm in the list](/KodeKloud/100%20Days/images/azure-day8/02.png)

Go to **Settings > Disks**.

![Disks page with no data disks attached](/KodeKloud/100%20Days/images/azure-day8/03.png)

Select **Attach existing disks**. A new row appears below, where I selected `nautilus-disk`.

![New data disk row](/KodeKloud/100%20Days/images/azure-day8/04.png)

![nautilus-disk selected](/KodeKloud/100%20Days/images/azure-day8/05.png)

Click **Apply** to attach it. After a few seconds it's fully attached to the VM.

Checking the disk itself, its state shows as **Attached**.

![Disk state Attached](/KodeKloud/100%20Days/images/azure-day8/06.png)

# Notes

Attaching the disk in Azure is only half the job on a real server. Inside the VM, a new data disk still has to be partitioned, formatted and mounted before anything can use it.
