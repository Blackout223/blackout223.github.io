---
layout: default
title: Day 1 - Create SSH Key Pair for Azure VM
---

# Overview

The Nautilus DevOps team is moving part of their infrastructure to Azure in small steps. For this task I had to create an SSH key pair:

- Name: `xfusion-kp`
- Type: `rsa`

# What is an Azure SSH key?

It works the same way as SSH keys anywhere else. You keep the private key on your machine and the public key goes on the Azure VM. Only someone with the private key can log in, so there's no password to guess or brute force.

# Solution

I searched for **SSH keys** in the portal.

![Searching for SSH keys](/KodeKloud/100%20Days/images/azure-day1/01.png)

Then clicked **Create**.

![Create button](/KodeKloud/100%20Days/images/azure-day1/02.png)

On the Basics tab I filled in:

- Subscription: the lab subscription
- Resource group: the existing one
- Region: left as the default
- Key pair name: `xfusion-kp`
- SSH key type: RSA

![Basics tab](/KodeKloud/100%20Days/images/azure-day1/03.png)

Then **Review + create**. Once validation passed, I clicked **Create** again.

![Validation passed](/KodeKloud/100%20Days/images/azure-day1/04.png)

Azure then prompts you to download the private key. This is the only chance to get it.

![Download private key](/KodeKloud/100%20Days/images/azure-day1/05.png)

After refreshing the portal the key shows as created.

![Key created](/KodeKloud/100%20Days/images/azure-day1/06.png)

# Notes

- Set the permissions on the private key before using it: `chmod 600 ~/.ssh/xfusion-kp.pem`
- Don't share the private key or store it anywhere public.
- Azure Key Vault is an option for storing keys centrally instead of on people's laptops.
