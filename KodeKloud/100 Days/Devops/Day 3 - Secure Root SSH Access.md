---
layout: default
title: Day 3 - Secure Root SSH Access
---

# Overview

After a security audit, the xFusionCorp Industries security team brought in new rules, including no direct root login over SSH. My task was to turn off root SSH login on all three app servers in the Stratos Datacenter.

If root can log in over SSH, an attacker only needs one password to own the whole server. It also means every admin action in the logs just says "root", so you can't tell who actually did it. With it turned off, people have to log in as themselves and then use sudo.

# Solution

These are the three app servers and their credentials:

<img width="1120" height="266" alt="App servers and credentials" src="https://github.com/user-attachments/assets/f9e3b504-fb17-467d-9936-76a88c15c9b3" />

**Nautilus App 1**

```sh
ssh tony@172.16.238.10
# password: Ir0nM@n
sudo su
vi /etc/ssh/sshd_config
```

`/etc/ssh/sshd_config` is the main SSH server configuration file. `PermitRootLogin` was set to `yes`, so I changed it to `no` and saved with `:wq`.

<img width="1281" height="1175" alt="PermitRootLogin set to yes" src="https://github.com/user-attachments/assets/c40aa443-2d99-4ed7-b323-cd6ffd0e6921" />
<img width="1414" height="1208" alt="PermitRootLogin changed to no" src="https://github.com/user-attachments/assets/31ee6098-fd16-478f-8e73-49d0077aa0a1" />

The change only takes effect once the SSH service is restarted:

```sh
systemctl restart sshd
```

**Nautilus App 2**

```sh
ssh steve@172.16.238.11
# password: Am3ric@
```

Same steps: switch to root, set `PermitRootLogin no`, then restart sshd.

<img width="1010" height="900" alt="App 2 sshd_config" src="https://github.com/user-attachments/assets/d8eced97-65b2-4e82-8401-57e43f8ea88c" />

**Nautilus App 3**

```sh
ssh banner@172.16.238.12
# password: BigGr33n
```

Same steps again.

<img width="809" height="1006" alt="App 3 sshd_config" src="https://github.com/user-attachments/assets/eddd99d8-9254-440a-afaa-1929629a8517" />

# Checking it worked

One thing to watch out for: if you edit the file and don't restart sshd, it carries on running with the old settings and root can still log in. When I first wrote this up I only mentioned the restart at the very end, so I've moved it to straight after each edit.

To check, this shows the setting sshd is actually running with, not just what's in the file:

```sh
sshd -T | grep -i permitrootlogin
# permitrootlogin no
```

Then try logging in as root. It should be refused:

```sh
ssh root@172.16.238.10
# Permission denied
```

# Doing this for real

Editing three servers by hand is fine for a lab, but with fifty servers you'd miss one. I'd do it with an Ansible task instead (I install Ansible on Day 8), and set `PermitRootLogin no` in the base image so new servers start out that way. I'd also turn off password login and only allow SSH keys.

It's also worth running `sshd -t` before restarting. It checks the config file for mistakes, so a typo doesn't stop sshd starting and lock everyone out.
