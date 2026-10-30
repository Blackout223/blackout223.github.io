---
layout: default
title: Day 1 - Create Key Pair
---

# Overview

The Nautilus DevOps team is moving part of their infrastructure to AWS in small steps. For this task I had to create a key pair:

- Name: `nautilus-kp`
- Type: `rsa`

# What is an AWS key pair?

A key pair is a public and private key used to SSH into EC2 instances. AWS keeps the public key and you download the private key. Only someone with the private key can log in, which is a lot safer than using passwords.

# Solution

I created the key pair from the CLI and saved the private key straight to a file:

```sh
aws ec2 create-key-pair \
  --key-name nautilus-kp \
  --key-type rsa \
  --query 'KeyMaterial' \
  --output text > nautilus-kp.pem
```

The `--query 'KeyMaterial'` part pulls out just the private key from the response, so the .pem file only contains the key and nothing else. AWS only gives you the private key once, so if you lose it you have to make a new key pair.

![Creating the key pair](/KodeKloud/100%20Days/images/aws-day1/01.png)

Then I checked it had been created:

```sh
aws ec2 describe-key-pairs --key-names nautilus-kp
```

This shows the key pair's ID, fingerprint and type.

![describe-key-pairs output](/KodeKloud/100%20Days/images/aws-day1/02.png)

Last, I locked down the permissions on the key:

```sh
chmod 400 nautilus-kp.pem
```

This makes it read-only for me and removes access for everyone else. SSH will refuse to use a private key that other users can read.

![chmod 400](/KodeKloud/100%20Days/images/aws-day1/03.png)

# Notes

- Never commit .pem files to Git. Add `*.pem` to `.gitignore`.
- Keep a backup somewhere encrypted, like a password manager.
- Rotate keys regularly and delete ones that aren't used anymore.
