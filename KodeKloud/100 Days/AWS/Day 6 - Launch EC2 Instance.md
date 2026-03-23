---
layout: default
title: Day 6 - Launch EC2 Instance
---

# Overview

For this task, create an EC2 instance with following requirements:

1) The name of the instance must be `datacenter-ec2`.

2) You can use the `Amazon Linux` AMI to launch this instance.

3) The Instance type must be `t2.micro`.

4) Create a new RSA key pair named `datacenter-kp`.

5) Attach the default (available by default) security group.


# What is Amazon EC2

Amazon EC2 is a service that provides resizable compute capacity in the cloud. Think of EC2 as virtual servers that you can launch in minutes, configure with your operating system and applications, and scale up or down based on your needs. EC2 gives you complete control over your computing resources and runs on Amazon's proven computing environment.
# Solution

Head over to EC2 > Instances - This is where we launch the instances
<img width="2538" height="638" alt="image" src="https://github.com/user-attachments/assets/29895768-03c3-4277-9cd6-01a508894bc3" />

On the EC2 Dashboard, **"Launch instance"** - This is where we will be able to configure our instance
<img width="1826" height="1163" alt="image" src="https://github.com/user-attachments/assets/d50a0303-3ef4-482b-b7d7-dea11f816e51" />

In the **"Name"** field, enter:`datacenter-ec2`

Amazon Linux is the default AMI and can be left

Under instance type select` t2.micro` from the dropdown
<img width="788" height="794" alt="image" src="https://github.com/user-attachments/assets/b620f441-49ca-4821-b83b-594c6e8c02ca" />

Now to create the RSA key pair, which needs to be named to `datacenter-kp`
<img width="592" height="566" alt="image" src="https://github.com/user-attachments/assets/a78d0c6e-d568-42b5-b698-a2778e1f52c3" />

Create key pair and it will automatically download our private key (Make sure to secure this as this is the only chance you get to download it)

If using Linux, you will need to set the correct permissions for the key if you need to SSH from your device. This can be done using this command
```sh
chmod 400 devops-kp.pem
```

For Windows:
```powershell
 icacls datacenter-kp.pem /inheritance:r
```

We can leave the other settings as default and can now launch our instance
<img width="1596" height="1088" alt="image" src="https://github.com/user-attachments/assets/b37b4cce-2a75-4eaa-b755-55d74ae0ad65" />

The instance has successfully been launched
<img width="1082" height="218" alt="image" src="https://github.com/user-attachments/assets/1a31d6c8-9828-4084-b7b0-f62c96b9bb7e" />

Now let's test our SSH key from earlier to SSH the instance

We will need the public IP address - Which is `98.92.225.33`
<img width="2287" height="848" alt="image" src="https://github.com/user-attachments/assets/c53ad8ea-625c-4dc4-881a-3b024f8d99c2" />

Now run:
```sh
ssh -i datacenter-kp.pem ec2-user@98.92.225.33
```

We have successfully logged in through SSH
<img width="819" height="264" alt="image" src="https://github.com/user-attachments/assets/6c135e78-39ec-472b-8287-3a3bec30f301" />
