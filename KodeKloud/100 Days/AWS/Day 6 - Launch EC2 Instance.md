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

Amazon EC2 is a service that provides resizable compute capacity in the cloud. Think of EC2 as virtual servers that you can launch in minutes, configure with your operating system and applications, and scale up or down based on your needs.
# Solution

Head over to EC2 > Instances - This is where we launch the instances
<img alt="EC2 Instances page" src="/KodeKloud/100%20Days/images/aws-day6/01.png" />

On the EC2 Dashboard, **"Launch instance"** - This is where we will be able to configure our instance
<img alt="Launch an instance" src="/KodeKloud/100%20Days/images/aws-day6/02.png" />

In the **"Name"** field, enter:`datacenter-ec2`

Amazon Linux is the default AMI and can be left

Under instance type select` t2.micro` from the dropdown
<img alt="Choosing t2.micro" src="/KodeKloud/100%20Days/images/aws-day6/03.png" />

Now to create the RSA key pair, which needs to be named to `datacenter-kp`
<img alt="Creating the datacenter-kp key pair" src="/KodeKloud/100%20Days/images/aws-day6/04.png" />

Create key pair and it will automatically download our private key (Make sure to secure this as this is the only chance you get to download it)

If using Linux, you will need to set the correct permissions for the key if you need to SSH from your device. This can be done using this command
```sh
chmod 400 datacenter-kp.pem
```

For Windows:
```powershell
 icacls datacenter-kp.pem /inheritance:r
```

We can leave the other settings as default and can now launch our instance
<img alt="Launch settings filled in" src="/KodeKloud/100%20Days/images/aws-day6/05.png" />

The instance has successfully been launched
<img alt="Launch succeeded" src="/KodeKloud/100%20Days/images/aws-day6/06.png" />

Now let's test our SSH key from earlier to SSH the instance

We will need the public IP address - Which is `98.92.225.33`
<img alt="datacenter-ec2 running with its public IP" src="/KodeKloud/100%20Days/images/aws-day6/07.png" />

Now run:
```sh
ssh -i datacenter-kp.pem ec2-user@98.92.225.33
```

We have successfully logged in through SSH
<img alt="SSH into the instance" src="/KodeKloud/100%20Days/images/aws-day6/08.png" />
