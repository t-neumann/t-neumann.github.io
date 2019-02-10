---
title: AWS architecture outline
description: Resources to consider for engineering pipelines on AWS
date: 2019-02-10 09:45
image:
  path: /assets/images/categories/aws.svg
categories:
  - Pipelines
tags:
- AMI
- AWS
- Containers
- Docker
- Nextflow
---

If you talk about the omni-present buzzword **cloud computing**, you will inevitably stumble over [Amazon Web Services <i class="fab fa-aws" aria-hidden="true"></i>](https://aws.amazon.com). Sounds super cool and everybody gets excited about it, but I for my part was simply overwhelmed by the amount of services and products available from the platform.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-architecture/AWSServices.png" alt="AWS Services">

The good news for us bioinformaticians is - and probably all cloud computing professionals handling on enterprise solutions are going to beat me for this statement - for setting up a proper and failsafe analysis pipeline with AWS, you only need a tiny fraction of those and can ignore the rest. In this post, I will walk you through the essential AWS building blocks I deem required for a basic bioinformatics processing pipeline, their characteristics, caveats and how they play together.

# AWS building blocks

If you are familiar with cluster computing environments, you should not have a hard time to find the same architecture principal when building your own custom cluster computing environment in the cloud with AWS. I will elaborate on those pieces I encountered when building up a basic processing pipeline:

- `S3` for storage of input and auxiliary (e.g. index) files
- `EBS` as local compute storage
- `AMI` Machine image (the operating system) to be run on your instances
- `EC2` instances that do the actual computation
- `ECS` to create your "software" from Docker containers to run on your instances
- `AWS Batch` that handles everything from submission to scaling and proper finalization of your individual jobs

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-architecture/AWSArchitecture.png" alt="AWS Architecture">

## S3 - Simple Storage Service

## EBS - Elastic Block Store

## AMI - Amazon Machine Image

## EC2 - Elastic Compute Cloud

## ECS - Elastic Container Service

## AWS Batch

# Putting it all together
