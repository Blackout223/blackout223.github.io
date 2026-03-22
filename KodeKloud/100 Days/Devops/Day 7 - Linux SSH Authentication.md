---
layout: default
title: Day 7 - Linux SSH Authentication
---

# Overview

The system admins team of `xFusionCorp Industries` has set up some scripts on `jump host` that run on regular intervals and perform operations on all app servers in `Stratos Datacenter`. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1). Based on the requirements, perform the following:  
  
Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.

# Solution

First is to create the public/private key pair on the jumphost
```bash
ssh-keygen -t rsa -b 4096
```
<img width="658" height="418" alt="image" src="https://github.com/user-attachments/assets/49cdc91d-0a74-49dd-90f9-5500af4c299d" />

**Command Breakdown:**

- `ssh-keygen`: SSH key generation utility
- `-t rsa`: Specifies RSA algorithm for key generation
- `-b 4096`: Sets key length to 4096 bits (enhanced security over default 2048)

Next is to copy the public key to each application server's authorized_keys file.

**App Server 1 Configuration**
```bash
ssh-copy-id tony@stapp01
```
<img width="1007" height="314" alt="image" src="https://github.com/user-attachments/assets/35122641-d783-4072-8ac6-6ba515a52ead" />

The `ssh-copy-id` utility automates the following process:

1. Reads the local public key (`~/.ssh/id_rsa.pub`)
2. Connects to the remote server via SSH (requires password this one time)
3. Creates `~/.ssh/` directory on remote server if it doesn't exist
4. Appends the public key to `~/.ssh/authorized_keys`
5. Sets correct permissions on remote directories and files

Now to test to see if the `thor` user can ssh into tony without a password
```sh
ssh tony@stapp01
```
<img width="408" height="79" alt="image" src="https://github.com/user-attachments/assets/c7c8d7b3-6c3d-4a42-a176-9793f5e473c2" />

It works. Now time to repeat the same steps with the other app servers

**App Server 2 Configuration**
```sh
ssh-copy-id steve@stapp02
```
```sh
ssh steve@stapp02
```
<img width="956" height="500" alt="image" src="https://github.com/user-attachments/assets/f6b40b90-9270-41c9-bb14-b489e1e239f9" />

**App Server 3 Configuration**

```sh
ssh-copy-id banner@stapp03
```
```sh
ssh banner@stapp03
```
<img width="1003" height="364" alt="image" src="https://github.com/user-attachments/assets/cdbe22e3-9f85-451f-8091-a631a915cf18" />

# Conclusion

Successfully implemented password-less SSH authentication from the jump host (`thor` user) to all three application servers in the Stratos Datacenter. The implementation enables the system administration team to run automated scripts across the infrastructure without manual intervention, directly supporting xFusionCorp Industries' automation initiatives.

