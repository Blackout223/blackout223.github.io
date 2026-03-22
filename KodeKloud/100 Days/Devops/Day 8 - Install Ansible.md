---
layout: default
title: Day 8 - Install Ansible
---

# Overview

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with `Ansible` for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use `jump host` as an Ansible controller to test different kind of tasks on rest of the servers.  

Install `ansible` version `4.9.0` on `Jump host` using `pip3` only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

# What is Ansible?

Ansible is an open-source automation platform that enables infrastructure as code, configuration management, and application deployment across multiple servers simultaneously using simple, human-readable YAML playbooks. It uses an agentless architecture, requiring only SSH access to managed nodes, making it lightweight and easy to implement compared to other automation tools.

# Solution

As Ansible needs to be installed globally, we will need to switch to `root` using `sudo su`

Next is to install Ansible with pip3 and version 4.9.0 
```sh
pip3 install ansible==4.9.0
```
<img width="1402" height="948" alt="image" src="https://github.com/user-attachments/assets/368406a3-fba0-43c9-a134-40b3ec413d23" />

After Ansible has finished installing, you can verify the version by running this command:
```sh
pip3 show ansible
```

Ansible with version 4.9.0 has now fully been installed. 

# Conclusion

Successfully installed Ansible version 4.9.0 on the jump host using pip3, establishing the foundation for the Nautilus DevOps team's automation and configuration management initiatives. The installation meets all specified requirements: deployed via pip3, accessible globally to all system users, and fully functional for automation workflows.

