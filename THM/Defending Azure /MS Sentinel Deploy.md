---
layout: default
title: MS Sentinel - Deploy
---

# MS Sentinel: Deploy

Setting up Sentinel for the first time, and the decisions you need to make before you do.

## Before deploying

Sentinel is a service, not a resource. It gets switched on for a Log Analytics workspace (LAW), and the LAW is the actual resource that stores the logs. So every Sentinel workspace is a LAW, but not every LAW has Sentinel on it.

The main design choice is how many workspaces to have:

- **One workspace for everything.** Everything in one place, but logs from other regions have to travel to it. That costs bandwidth and can break data residency rules, which matters a lot in regulated industries like finance.
- **One workspace per region.** Fixes the residency and bandwidth problem, but you lose the single view and have more access and retention settings to manage.
- **Multi-tenant.** For when you're monitoring other organisations' tenants, like an MSSP would.

The workspace region can't be changed once it's created, so it needs deciding up front.

## Creating the workspace and enabling Sentinel

You need the Microsoft Sentinel Contributor role on the resource group to do this.

I searched for Sentinel in the portal and selected **Create Microsoft Sentinel**.

<img alt="Searching for Microsoft Sentinel" src="https://github.com/user-attachments/assets/94ca2ca1-3be5-4dbe-8abb-2ff835025f64" />

<img alt="Create Microsoft Sentinel" src="https://github.com/user-attachments/assets/93ae097d-7260-41e5-afe2-356182f21acb" />

There was no workspace yet, so I created a new one.

<img alt="Create a new workspace" src="https://github.com/user-attachments/assets/faf1f47d-fecf-4c32-98e1-b77f20d75f96" />

I picked the subscription and resource group, named the workspace and chose the region.

<img alt="Workspace details" src="https://github.com/user-attachments/assets/3aeecc06-9a2d-42d8-bd88-d79c0f9966fc" />

Then added Sentinel to the new workspace.

<img alt="Adding Sentinel to the workspace" src="https://github.com/user-attachments/assets/a7603e95-1a32-4742-bf5c-127c0c6b6d3b" />

After a few minutes Sentinel was ready.

<img alt="Sentinel ready" src="https://github.com/user-attachments/assets/21d4e014-4f35-471b-b876-c7457ecea1cf" />

## Roles

Sentinel has its own built-in roles, and each one builds on the one before:

- **Reader:** view data, incidents and workbooks. For managers and stakeholders.
- **Responder:** Reader plus managing incidents (assigning, changing status). Usually L1 analysts.
- **Contributor:** Responder plus creating rules, workbooks and installing content. Usually L2 or security engineers.
- **Playbook Operator:** run playbooks manually.

These should be assigned at the resource group level, because Sentinel relies on other resources in the same group.

## Data retention

Some settings live in Sentinel and some in the underlying workspace. Retention is in the workspace settings, under **Usage and estimated costs**.

<img alt="Sentinel settings" src="https://github.com/user-attachments/assets/bdc26f16-e0e0-47d7-9d3f-5f0e3876a512" />

<img alt="Workspace settings" src="https://github.com/user-attachments/assets/c6c7f642-31e8-4179-b2c9-c579ddb6fcf5" />

<img alt="Usage and estimated costs" src="https://github.com/user-attachments/assets/bf7abb9b-26a8-4399-8860-528260a6d1cd" />

The default is 31 days. Compliance often needs longer, but Sentinel is billed on how much data you ingest and keep, so longer retention costs more. It's worth checking the usage figures regularly so the bill doesn't creep up.

<img alt="Log usage" src="https://github.com/user-attachments/assets/f0664508-2f44-402a-be45-0fc14b9bb917" />
