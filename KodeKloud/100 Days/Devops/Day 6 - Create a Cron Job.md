---
layout: default
title: Day 6 - Create a Cron Job
---

# Overview

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:  
  
a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.  
  
b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

# Solution

We will need to access each application server via SSH through the jump host:
```bash
ssh tony@stapp01  
ssh steve@stapp02 
ssh banner@stapp03 
```

Next, change to root using `sudo su`

## What is Cronie? 

- `cronie` is the modern cron implementation for RHEL-based systems
- Provides the `crond` daemon for scheduling tasks
- Required for time-based job scheduling

To install `cronie` - Run:
```sh
yum install -y cronie
```
<img width="1412" height="900" alt="image" src="https://github.com/user-attachments/assets/3f585cc6-8e28-4808-8739-0e7c4d5d909e" />
<img width="1404" height="894" alt="image" src="https://github.com/user-attachments/assets/2a27ed2d-dc94-4399-a56d-da3b4989c481" />
<img width="1408" height="899" alt="image" src="https://github.com/user-attachments/assets/0f2bceb4-a9a1-4942-8f77-880ea9cd20f0" />

Next step is to start and enabled and start the `crond` service
```bash
systemctl enable crond
systemctl start crond
```
<img width="928" height="387" alt="image" src="https://github.com/user-attachments/assets/6f8a332d-6acb-4423-a011-e67fff44075e" />
<img width="909" height="406" alt="image" src="https://github.com/user-attachments/assets/48b03313-32a1-403a-8233-feabe2f68336" />
<img width="897" height="406" alt="image" src="https://github.com/user-attachments/assets/80d02e9c-6a39-4ce1-9939-56c0f84adecc" />

Now that the service has been enabled the next step is to add the scheduled task to root's crontab
```sh
(crontab -l 2>/dev/null; echo "*/5 * * * * echo hello > /tmp/cron_text") | crontab -
```

The cronjob runs every 5 minutes and adds the `cron_text` to the `/tmp` directory with the contents of `hello` inside the file

To verify the crontab has been modified, `crontab -l` can be run to check the changes, which shows it has been updated
<img width="932" height="87" alt="image" src="https://github.com/user-attachments/assets/f53d6f39-3288-4e59-ae2a-d773517f65fd" />
<img width="892" height="78" alt="image" src="https://github.com/user-attachments/assets/4587c2a8-9438-4b77-80f5-dd39b899344a" />
<img width="890" height="59" alt="image" src="https://github.com/user-attachments/assets/ca1f8d11-6bed-4a67-8773-a65046217143" />

Each app server now has the updated cronjob. The `crond` service will need to be restarted, once this has been added
```sh
systemctl restart crond
```

After 5 minutes, the file will appear in the `/tmp` directory
<img width="369" height="117" alt="image" src="https://github.com/user-attachments/assets/84f662f7-130a-47eb-9700-607c02bd01be" />

# Conclusion

This project demonstrated practical Linux system administration skills including package management, service configuration, and task scheduling across a multi-server environment. The implementation provided hands-on experience with real-world infrastructure automation scenarios commonly encountered in DevOps roles
