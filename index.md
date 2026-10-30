---
layout: default
title: Blackout | Cybersecurity/Cloud Security Portfolio
description: Five years in IT support and security operations, now building towards a Cloud Security Engineer role
---

## About me

I'm a Technical Support Representative at TryHackMe. Part of my role is investigating AWS abuse alerts using CloudTrail, VPC Flow Logs and Elastic SIEM, and I lead the company's bug bounty programme. Before TryHackMe I worked in IT support for companies in the financial sector, where I looked after Microsoft 365, Entra ID and Intune, helped roll out EDR, and worked on Cyber Essentials Plus and ISO 27001.

I'm looking for a Cloud Security Engineer role. AWS is my main platform and I'm open to Azure and GCP.

[GitHub](https://github.com/Blackout223) · [TryHackMe](https://tryhackme.com/p/Blackout)

---

## Featured projects

### [BillHound](https://github.com/Blackout223/billhound)

When someone steals AWS keys, it often shows up in the bill before anything else. Things like new regions being used, GPU instances for cryptomining, or a big jump in data going out. BillHound goes through AWS billing exports (Cost and Usage Reports) looking for those signs and ranks what it finds by severity. It doesn't need any AWS credentials, and it can be run as a check in a CI pipeline.

Python, pytest, GitHub Actions

### [Raspberry Pi Cloud Security Lab](https://github.com/Blackout223/Pi-Container-Security)

A k3s cluster on a Raspberry Pi 4. In each stage I set something up, misconfigure it on purpose, then work out how to spot the problem and fix it. Done so far: hardening a container and scanning it with Trivy, then RBAC, network policies and Pod Security Standards in Kubernetes. Falco, Kyverno and kube-bench are next.

Kubernetes (k3s), Docker, Trivy

### [AWS Threat Detection and Automated Response](AWS%20Projects/Threat%20Detection%20and%20Automated%20Response%20Pipeline)

Threat detection for a made-up fintech company. CloudWatch alarms go off for root logins, console logins without MFA and IAM changes. GuardDuty findings go through EventBridge, and a Lambda turns off any access key GuardDuty flags as compromised. I tested it by setting off the alarms myself and with GuardDuty's sample findings, and wrote up what I'd do differently.

CloudTrail, GuardDuty, EventBridge, Lambda, SNS, Security Hub

---

## Certifications

- AWS Certified Cloud Practitioner (2025 to 2028)
- AWS Solutions Architect Associate, in progress

---

## Hands-on practice

Write-ups from labs, CTFs and guided projects I've done.

- [TryHackMe write-ups](thm)
- [KodeKloud 100 Days of DevOps](kodekloud)
- [NextWork AWS projects](nextwork)
