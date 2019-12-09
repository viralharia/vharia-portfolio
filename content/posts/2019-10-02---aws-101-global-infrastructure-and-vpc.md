---
title: AWS 101 - Global Infrastructure of AWS - Region and Availability Zone(Az)
date: '2019-10-02 14:45'
template: "post"
draft: false
slug: "/posts/aws-101-global-infrastructure-and-vpc/"
category: "AWS"
tags:
  - "AWS"
  - "AWS101"
description:
socialImage: "/media/image-2.jpg"
---
## AWS Region
* An AWS Region is a geographical location. Every region is physically isolated from and independent of every other region in terms of location, power, water supply, etc.
* Inside each region, there are two or more availability zones with each AZ hosted in separate data centers from another zone.
* The level of region isolation is critical for workloads with compliance and data sovereignty requirements where guarantees must be made that user data does not leave a particular geographic region. 
* The presence of AWS regions worldwide is also important for workloads that are latency-sensitive and need to be located near users in a particular geographic area.
* When you create certain resources in a region, you will be asked to choose a zone in which to host that resource.

## AWS availability zones
* An availability zone is a **_logical data center_** in a region.
* logical because there can be multiple data center in 1 AZ.
* In each AZ, all data centers are connected to each other over redundant low-latency private network links. 
* Likewise, all AZ in a region communicate with each other over redundant private network links. These intra and inter-zone links are heavily used for data replication by a number of AWS services including storage and managed databases.
* AZ is an important and foundational concept for building a highly available, fault-tolerant application using AWS.

## Why are availability zones such an important and foundational concept in Amazon Web Services?
![single AZ architecture diagram](/media/az3.png)

* The diagram above illustrates a region with 2 zones where only one is being utilized. The architecture mirrors what a typical 3 tier application running in a user's single on-premises data center may look like. While there are redundant servers running in each tier, the data center itself is a single point of failure.

![single AZ architecture diagram](/media/az4.png)
* In contrast to the earlier architecture, the diagram above illustrates the recommended practice of spanning an application across multiple zones.
* By placing virtual servers for each tier in each zone, users are able to eliminate a single point of failure.
* Amazon Elastic Load Balancers situated at start ensures that even if an entire zone goes offline, traffic will be directed to the appropriate one.
* **It’s worth pointing out that the ELBs “live” outside the zones and are therefore not impacted by the failure of any particular one. ELB is one of many AWS services that have a regional scope and can span across zones in a given region.**
