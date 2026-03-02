---
layout: default
title: Day 4 - Script Execution Permissions
---

# Overview

In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on `App Server 2` within the Stratos Datacenter.  

Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 2`. Additionally, ensure that all users have the capability to execute it.

# Solution

From the previous days, we already know the username and password to `APP Server 2`

```sh
ssh steve@172.16.238.11
# Password: Am3ric@
```

Now switch to root
```bash
sudo su
```
<img width="810" height="350" alt="image" src="https://github.com/user-attachments/assets/4cb623d6-bcc2-4608-ae73-e23356cab16b" />

Now head to `/tmp` directory, which is where our bash script is stored
```sh
cd /tmp
```

Running `ls -l` will list all the files in the directory and the permissions.  Which the script has no permissions (`----------` - This means no permissions on the file)
<img width="1095" height="124" alt="image" src="https://github.com/user-attachments/assets/1c8a1678-77ba-4ddb-a023-b714bd0fcecf" />

To make the file executable, the command `chmod` will need to be run as this changes access permissions.
```sh
chmod +x xfusioncorp.sh
```

This command will also need to be run so it has read/write/execute permissions for all users to be able to execute it
```sh
chmod 755 xfusioncorp.sh
```

755 means the owner has full control, while everyone else can read and run the file but not modify it

**What Each Number Means:**

**7 (Owner):**

- 4 (read) + 2 (write) + 1 (execute) = **7**
- Can read, modify, and run the file

**5 (Group):**

- 4 (read) + 0 (write) + 1 (execute) = **5**
- Can read and run, but cannot modify

**5 (Others):**

- 4 (read) + 0 (write) + 1 (execute) = **5**
- Can read and run, but cannot modify

The script now has executable/read/write permissions
<img width="1063" height="79" alt="image" src="https://github.com/user-attachments/assets/2577f343-2a58-43e0-ba6a-89ac29b5514f" />

Before executing the script, lets look at the contents of what the script does
```sh
cat xfusioncorp.sh
```
<img width="487" height="93" alt="image" src="https://github.com/user-attachments/assets/072b75d6-59c8-4a53-a86a-79d7d19ed04c" />

Checking the contents, the script will output this in the terminal when executed
`Welcome To KodeKloud`

Let's now execute the script, which can be done with this command:
```sh
./xfusioncorp.sh
```
<img width="373" height="76" alt="image" src="https://github.com/user-attachments/assets/2507cb5a-c804-41f9-a7df-4dd6bee2f915" />

