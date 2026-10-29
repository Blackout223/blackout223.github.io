---
layout: default
title: Blackout | Cloud Security Portfolio
description: Projects and write-ups in AWS detection, incident response and container security
---

## About me

I'm a Technical Support Representative at TryHackMe. Part of my role is investigating AWS abuse alerts using CloudTrail, VPC Flow Logs and Elastic SIEM, and I lead the company's bug bounty programme. Before that I spent five years in IT support and security operations in regulated financial-sector environments, working with Microsoft 365, Entra ID, Intune, EDR rollouts, Cyber Essentials Plus and ISO 27001.

I'm looking for a Cloud Security Engineer role. AWS is my main platform and I'm open to Azure and GCP.

[GitHub](https://github.com/Blackout223) · [TryHackMe](https://tryhackme.com/p/Blackout)

---

## Featured projects

### [CostCanary](https://github.com/Blackout223/costcanary)

Attackers who steal AWS keys usually leave a trace in the bill before anyone notices: new regions, GPU instances, Bedrock usage, data egress spikes. CostCanary reads Cost and Usage Report data and flags those signs, ranked by severity with a plain-English reason for each. It needs no AWS credentials and can fail a CI pipeline on high-severity findings.

Python, pytest, GitHub Actions

### [Raspberry Pi Cloud Security Lab](https://github.com/Blackout223/Pi-Container-Security)

A k3s cluster on a Raspberry Pi 4. In each stage I set something up, break it the way real systems get misconfigured, then detect and fix it. So far: container hardening and Trivy triage, then RBAC, network policies and Pod Security Standards. Falco, Kyverno and kube-bench are next.

Kubernetes (k3s), Docker, Trivy

### [AWS Threat Detection and Automated Response](AWS%20Projects/Threat%20Detection%20and%20Automated%20Response%20Pipeline)

A detection pipeline for a fictional fintech: CloudTrail and CloudWatch alarms for root use, logins without MFA and IAM changes, GuardDuty findings routed through EventBridge, and a Lambda that disables compromised access keys. Tested with simulated attacks and GuardDuty sample findings.

CloudTrail, GuardDuty, EventBridge, Lambda, SNS, Security Hub

---

## Certifications

- AWS Certified Cloud Practitioner (2025 to 2028)
- AWS Solutions Architect Associate, in progress

---

## Hands-on practice

Labs, challenges and guided projects I have worked through, with my write-ups.

- [TryHackMe write-ups](thm)
- [KodeKloud 100 Days of DevOps](kodekloud)
- [NextWork AWS projects](nextwork)
