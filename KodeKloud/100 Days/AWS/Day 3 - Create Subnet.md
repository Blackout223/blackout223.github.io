---
layout: default
title: Day 3 - Create Subnet
---

# Overview

For this task I had to create one subnet named `nautilus-subnet` in the default VPC.

# What is a VPC?

A VPC (Virtual Private Cloud) is your own isolated network inside AWS. You choose the IP range, and create subnets, route tables and gateways. Your resources are kept separate from other AWS customers.

# What is a subnet?

A subnet is a slice of the VPC's IP range. Subnets live in one Availability Zone, and you use them to group resources and give each group different routing and security rules. For example, web servers in a public subnet and databases in a private one.

# Solution

I searched for VPC in the console.

![Searching for VPC](/KodeKloud/100%20Days/images/aws-day3/01.png)

Then went to **Subnets > Create subnet**. The default subnets are already listed here.

![Subnets page](/KodeKloud/100%20Days/images/aws-day3/02.png)

I picked the default VPC, named the subnet `nautilus-subnet`, and set the subnet CIDR block to `172.31.96.0/24`. The default VPC uses `172.31.0.0/16`, so the subnet has to be inside that range and can't overlap the existing default subnets. A /24 gives 256 addresses, and AWS reserves 5 of them in every subnet.

![Subnet settings](/KodeKloud/100%20Days/images/aws-day3/03.png)

The subnet was created.

![Subnet created](/KodeKloud/100%20Days/images/aws-day3/04.png)

# Notes

- In a real setup you'd create subnets in at least two Availability Zones so one AZ going down doesn't take everything with it.
- Turning on VPC Flow Logs lets you see what traffic is going in and out. I use flow logs at work when looking into AWS abuse reports.
- Network ACLs can add an extra layer on top of security groups at the subnet level.
