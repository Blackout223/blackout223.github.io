---
layout: default
title: Day 4 - Enable Versioning for S3 Bucket
---

# Overview

Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption. The DevOps team has received a requirement for implementing such measures for one of the S3 buckets they are managing.

The s3 bucket name is `xfusion-s3-22939`, enable `versioning` for this bucket.

# What is Amazon S3?

Amazon S3 (Simple Storage Service) is an object storage service that offers industry-leading scalability, data availability, security, and performance. S3 allows you to store and retrieve any amount of data from anywhere on the web. It's designed for 99.999999999% (11 9's) durability and stores data across multiple Availability Zones automatically.

## What is S3 Versioning?

S3 Versioning is a feature that keeps multiple variants of an object in the same bucket. When enabled, S3 automatically creates and stores all versions of an object (including all writes and even deletes). This provides protection against accidental deletions and overwrites, allowing you to recover previous versions of your data.

# Solution

Search for S3 
<img width="896" height="176" alt="image" src="https://github.com/user-attachments/assets/634e6c2a-b6cf-458c-bda2-a4c437f26aa0" />

The bucket will be listed here
<img width="1106" height="294" alt="image" src="https://github.com/user-attachments/assets/3f946f71-1f22-46b9-88dd-49cd9ade280d" />

Now, head into the bucket, where we will see a list of objects, properties and etc.
<img width="1776" height="439" alt="image" src="https://github.com/user-attachments/assets/0a0786e1-84a0-4dc7-b787-485ec73aff1c" />

Select properties, this is where versioning can be enabled
<img width="1833" height="1166" alt="image" src="https://github.com/user-attachments/assets/2009f5e9-415d-4e9b-b6d7-bddd67d1c89a" />

Select Edit and then enable Bucket Versioning and save changes
<img width="1693" height="479" alt="image" src="https://github.com/user-attachments/assets/cf8ac5df-e4b9-4176-a2b7-a57c2144cb79" />

The bucket now has versioning enabled
<img width="1741" height="507" alt="image" src="https://github.com/user-attachments/assets/813ce99b-e290-4890-9321-eff3c1eb0ee1" />
