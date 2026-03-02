---
layout: default
title: Day 3 - Secure Root SSH Access
---

# Overview

Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.  

Your task is to disable direct SSH root login on all app servers within the `Stratos Datacenter`.

# Solution

We have 3 app servers where we will need to disable SSH. These are the servers and credentials where we will need to disable SSH
<img width="1120" height="266" alt="image" src="https://github.com/user-attachments/assets/f9e3b504-fb17-467d-9936-76a88c15c9b3" />


Let's login to `Nautilus App 1` first and disable SSH
```sh
ssh tony@172.16.238.10
```

Enter password:
`Ir0nM@n`

We will need to switch to root using
`sudo su`

Now we will need to edit the file `/etc/ssh/sshd_config` - As this is the main configuration file for SSH

We can us `vi` to edit the file 
`vi /etc/ssh/sshd_config`

`PermitRootLogin` is set to yes and will need to be changed to no
<img width="1281" height="1175" alt="image" src="https://github.com/user-attachments/assets/c40aa443-2d99-4ed7-b323-cd6ffd0e6921" />
<img width="1414" height="1208" alt="image" src="https://github.com/user-attachments/assets/31ee6098-fd16-478f-8e73-49d0077aa0a1" />

Now we can exit and save the file by typing  `:wq`

Now we repeat the same steps for `Nautilus App 2/3`

```sh
ssh steve@172.16.238.11
```

Enter password:
`Am3ric@`

Switch to root and edit the file changing `PermitRootLogin` to no
<img width="1010" height="900" alt="image" src="https://github.com/user-attachments/assets/d8eced97-65b2-4e82-8401-57e43f8ea88c" />

Now we can disable on the final app server
```sh
ssh banner@172.16.238.12
```

Enter password:
`BigGr33n`

Switch to root and update the file
<img width="809" height="1006" alt="image" src="https://github.com/user-attachments/assets/eddd99d8-9254-440a-afaa-1929629a8517" />

All SSH access to login to root is now disabled. 

When making changes to the file, you will need to restart the SSH service each time for each server for the changes to take place. This can be done by running
```sh
systemctl restart sshd
```

