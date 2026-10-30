---
layout: default
title: Day 3 - Secure Root SSH Access
---

# Day 3 - Secure Root SSH Access

## Scenario

After a security audit, the xFusionCorp Industries security team has rolled out new rules, including a ban on direct root SSH login. My task was to disable direct root SSH login on all three app servers in the Stratos Datacenter.

## Why it matters

If root can log in over SSH, an attacker only needs one password to get full control of a server, and every admin action shows up in the logs as "root", so you can't tell who did what. Disabling it forces people to log in as their own named user and then use sudo, which gives you an audit trail and one more barrier for an attacker.

## What I did

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

## What went wrong

The easy mistake here is editing the file and moving on without restarting sshd. Until the service restarts it keeps running with the old settings, so root login is still allowed even though the file says otherwise. My first version of this write-up only mentioned the restart at the very end. Restarting straight after each edit, then verifying, avoids this.

## How I verified it

On each server, check the setting sshd is actually running with, not just what the file says:

```sh
sshd -T | grep -i permitrootlogin
# permitrootlogin no
```

Then try logging in as root from another host. It should be refused:

```sh
ssh root@172.16.238.10
# Permission denied
```

## How I'd do this in production

Editing three servers by hand doesn't scale, and it's easy to miss one. In a real environment I would:

- Make the change with one Ansible task run against every server, so it's repeatable and in version control.
- Bake `PermitRootLogin no` into the base server image, so new servers are secure from the start.
- Disable password authentication as well and use SSH keys only.
- Run `sshd -t` to check the config is valid before restarting, so a typo doesn't lock everyone out.
