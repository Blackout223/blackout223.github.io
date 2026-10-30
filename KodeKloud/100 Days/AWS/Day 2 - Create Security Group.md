---
layout: default
title: Day 2 - Create Security Group
---

# Overview

Next step in the Nautilus team's move to AWS. For this task I had to create a security group in the default VPC:

- Name: `xfusion-sg`
- Description: `Security group for Nautilus App Servers`
- Inbound HTTP on port 80 from `0.0.0.0/0`
- Inbound SSH on port 22 from `0.0.0.0/0`

# What is a security group?

A security group is a virtual firewall for EC2 instances. You only write allow rules. By default all inbound traffic is blocked and all outbound traffic is allowed. Security groups are stateful, so if a request is allowed in, the reply is allowed back out automatically.

# Solution

I went to **EC2 > Network & Security > Security Groups**.

![Security Groups page](/KodeKloud/100%20Days/images/aws-day2/01.png)

Then clicked **Create security group**.

![Create security group](/KodeKloud/100%20Days/images/aws-day2/02.png)

I filled in the name and description, then added the two inbound rules: HTTP with the source set to Anywhere, and SSH with the source set to Anywhere.

![Inbound rules](/KodeKloud/100%20Days/images/aws-day2/03.png)

After creating it, the security group shows up in the console.

![Security group created](/KodeKloud/100%20Days/images/aws-day2/04.png)

# Notes

The lab asked for SSH open to `0.0.0.0/0`, which means anyone on the internet can try to log in. I wouldn't do that on a real server. HTTP open to everyone is fine for a public website, but SSH should be locked down to:

- your own IP or your company's IP range
- or a bastion host
- or no SSH at all, using AWS Systems Manager Session Manager instead
