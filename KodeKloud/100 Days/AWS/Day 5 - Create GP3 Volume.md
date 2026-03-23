---
layout: default
title: Day 5 - Create GP3 Volume
---

# Overview

Create a volume with the following requirements:

- Name of the volume should be `xfusion-volume`.
  
- Volume `type` must be `gp3`.
  
- Volume `size` must be `2 GiB`.

# What is Amazon EBS?

Amazon EBS (Elastic Block Store) is a high-performance block storage service designed for use with Amazon EC2 instances. Think of EBS as a virtual hard drive that you can attach to your EC2 instances. EBS volumes persist independently from the life of an EC2 instance, meaning your data remains intact even if the instance is stopped or terminated.

## Types of EBS Volumes

AWS offers a variety of EBS volumes as they have different purposes depending on the needs.

|Type|Name|IOPS|Throughput|Size|Use Case|Price|
|---|---|---|---|---|---|---|
|**gp3**|General Purpose SSD|3,000-16,000|125-1,000 MB/s|1 GiB - 16 TiB|Most workloads ✓|$0.08/GB-month|
|**gp2**|General Purpose SSD|3-16,000|Up to 250 MB/s|1 GiB - 16 TiB|Legacy, older apps|$0.10/GB-month|
|**io2**|Provisioned IOPS SSD|100-256,000|Up to 4,000 MB/s|4 GiB - 64 TiB|Critical databases|$0.125/GB-month|
|**io1**|Provisioned IOPS SSD|100-64,000|Up to 1,000 MB/s|4 GiB - 16 TiB|Legacy IOPS|$0.125/GB-month|
|**st1**|Throughput Optimized HDD|N/A|Up to 500 MB/s|125 GiB - 16 TiB|Big data, data warehouses|$0.045/GB-month|
|**sc1**|Cold HDD|N/A|Up to 250 MB/s|125 GiB - 16 TiB|Infrequent access|$0.015/GB-month|

# Solution

Head over to EC2 > Volumes - This is where we will be able to create and manage our EBS volumes
<img width="2548" height="756" alt="image" src="https://github.com/user-attachments/assets/6055b05f-5a87-40e6-8353-767698da38f1" />

Click `Create volume` - This will allow us to configure our Volume settings
<img width="1704" height="1057" alt="image" src="https://github.com/user-attachments/assets/6bbf32e4-5437-4175-9271-509aca61e2e1" />

Change `Volume Type` to gp3, which it will already be set as this is the default

In the **"Size (GiB)"** field, enter: `2`

The IOPS can be left as the default, same with the Throughput

Select any Availability Zone. It's important to note that volumes must be in same AZ as instances you'll attach to as you can not attach to instances in different AZ's

The Snapshot ID and Encryption can also be ignored. Although, you would want Encryption in a real world scenario.

Next, create the tags:
- Key - `Env`
- Value - `xfusion-volume`

The volume settings have been configured and the volume can now be created
<img width="1763" height="1162" alt="image" src="https://github.com/user-attachments/assets/55578e94-4137-4119-a534-cbe2e4b073db" />

The Volume has now been created with the `gp3` type
<img width="2268" height="1108" alt="image" src="https://github.com/user-attachments/assets/884817f4-b9ff-4178-b523-10716d5a1801" />
