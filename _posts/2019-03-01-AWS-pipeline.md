---
title: Pipelines on AWS
description: Setting up and running a pipeline on AWS
date: 2019-03-03 21:51
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
toc: true
toc_sticky: true
author_profile: false
---

The prerequisite for this post is that you know all the essential AWS building blocks and basic architecture of an AWS based batch scheduler as I presented in my [previous post](https://t-neumann.github.io/pipelines/AWS-architecture/). In this post, I will show you what environment and resources you have to actually set up on AWS to make an example pipeline run and then how to actually run jobs on the setup AWS Batch queue with [Nextflow](https://www.nextflow.io/).

## Credits

Many people have done a great job into setting up tutorials and blogs on this and I would like to acknowledge a few that helped me a lot to actually make my AWS pipelines happen:

* [Maxime Garcia](https://maxulysse.github.io/) and his great blog
* [Alex Peltzer](https://apeltzer.github.io/)
* [Paolo Di Tommaso](https://github.com/pditommaso) for Nextflow and Gitter support

There are a couple of tutorials that helped a lot:

* [Nextflow documentation](https://www.nextflow.io/docs/latest/awscloud.html#aws-batch)
* [Nextflow blog](https://www.nextflow.io/blog/2017/scaling-with-aws-batch.html)

## Prerequisites

### Accounts, users, roles, permissions

Some things have to be setup prior to starting setting up the actual AWS compute environment such as obvious things as an `AWS account` and other things like setting up an `IAM user` or `Service roles` which all has to be done only once and is exhaustively covered already in several blog posts such as [this one](https://apeltzer.github.io/post/01-aws-nfcore/) by Alex Peltzer and Tobias Koch. Therefore, I will not spend any time on this and suggest you just follow the instructions in the blog post until it is time to set up your `AMI` which is where I will start off.

## Salmon pipeline

### Overview

We would like to create a simple pipeline utilizing [Salmon](https://combine-lab.github.io/salmon/) applying a *quasi-mapping* approach to directly quantify transcripts from raw reads in fastq-files. *Salmon* requires an index **once** to be built which can then be used for any subsequent analysis of fastq-files and we are interested in the transcript quantification output (a plain text table) as well as pseudo-BAM files containing reads aligning to a given gene set of interest.

### AWS context

## Step 1: Creating a suitable AMI

I found the setup and configuration of suitable `AMIs` to be the most demanding step when creating an environment to run a pipeline on `AWS`. Several things have to be considered:

* Base image: It has to be `ECS`-compatible
* `EBS` storage: The attached volumes have to be large enough to contain all input, index, temporary and output files
* `AWS CLI`: The `AMI` has to contain the `AWS CLI` or otherwise no files can be fetch from and copied to `S3` from the `EBS` volume
* `AMIs` cannot be reused for processes containing less `EBS` (more is possible)

This section covers how you can set up your `AMI` for a given task of your pipeline and what to consider on the way.

### Choose an Amazon Machine Image (AMI)

As a first step, we want to make sure to pick a base image that supports `ECS` from the AWS Market Place. I strongly advise you to use one of the `Amazon ECS-Optimized Amazon Linux AMI` images.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Choose-AMI.png" alt="Choose AMI">

### Choose an Instance Type

The `EC2` instance we want to use to create our custom `AMI` does not need be resourceful, since we won't run any jobs on that. Therefore, a `t2.micro` instance is more than sufficient.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Choose-Instance.png" alt="Choose Instance">

### Configure Instance Details

The instance configuration can be mostly left to the defaults. However, I would strongly advise you to set the shutdown behaviour to `terminate`, otherwise attached volumes will be kept persistent and you continue to pay unless you explicitely terminate the instance manually. I actually ran into huge costs when misconfiguring this (300$) so **watch out!**.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Configure-Instance.png" alt="Configure Instance">

### Add storage

This is the single most important point of the entire `AMI` setup process - here you define the **minimum** number of added storage for your `AMI`. This storage **must** be large enough, to contain **all** input and index files for a given task as well as **all** temporary and final output files produced during the computation. I hope you did some thorough benchmarking and extrapolation of resources on your input dataset.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Add-Storage.png" alt="Add Storage">

### Add tags

Unless you want to add optional tags, nothing to do here...

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Add-Tags.png" alt="Add Tags">

### Configure Security Group

Before firing up your instance, you need to configure the associated security group. For me, letting AWS create the security group worked perfectly fine, I would still double check that you can connect to the `EC2` instance - in case of doubt set the source to `0.0.0.0/0`, even though probably all IT security experts will kill me for that. Now you are ready to **lauch the instance**.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Security-Group.png" alt="Security Group">

### SSH connect to instance

Now right click and hit *Connect* to get your `ssh` connect command to your instance. You might have to change the default `root` user to `ec2-user`.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-SSH.png" alt="AMI SSH connect">

### Adjust Docker container size to EBS

The first thing we want to check once we connected to our instance is that the Docker configuration reflects the amount of added EBS storage.

```bash
[ec2-user@ip-172-31-40-128 ~]$ docker info | grep -i data
 Data Space Used: 309.3MB
 Data Space Total: 42.42GB
 Data Space Available: 42.11GB
 Metadata Space Used: 4.833MB
 Metadata Space Total: 46.14MB
 Metadata Space Available: 41.3MB
```

In the above example we see that indeed Docker is configure for the specified 40 GB EBS data volume.

As per default, the maximum storage size of a single Docker container is 10 GB - independent of the data space available - we have to adjust this.

```bash
[ec2-user@ip-172-31-40-128 ~]$ docker info | grep -i base
 Base Device Size: 10.74GB
```

To this end, we have to extend the file in `/etc/sysconfig/docker-storage` to contain the following parameter `--storage-opt dm.basesize=40GB` and restart the Docker service.

```bash
sudo vi /etc/sysconfig/docker-storage
```
```bash
DOCKER_STORAGE_OPTIONS="--storage-driver devicemapper --storage-opt dm.thinpooldev=/dev/mapper/docker-docker--pool --storage-opt dm.use_deferred_removal=true --storage-opt dm.use_deferred_deletion=true --storage-opt dm.fs=ext4 --storage-opt dm.use_deferred_deletion=true --storage-opt dm.basesize=40GB"
```
```bash
[ec2-user@ip-172-31-40-128 ~]$ sudo service docker restart
Stopping docker:                                           [  OK  ]
Starting docker:       	.                                  [  OK  ]
```
```bash
[ec2-user@ip-172-31-40-128 ~]$ docker info | grep -i base
 Base Device Size: 42.95GB
```

### Install AWS CLI

`Nextflow` requires the `AWS CLI` to copy files such as input files and indices from and output files to `S3`.

Use the following lines to add it to your `AMI`:

```bash
sudo yum install -y bzip2 wget
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -f -p $HOME/miniconda
$HOME/miniconda/bin/conda install -c conda-forge -y awscli
rm Miniconda3-latest-Linux-x86_64.sh
```

Give it a quick spin to see whether everything is ok.

```bash
[ec2-user@ip-172-31-40-128 ~]$ ./miniconda/bin/aws --version
aws-cli/1.16.121 Python/3.7.1 Linux/4.14.94-73.73.amzn1.x86_64 botocore/1.12.111
```

### Save your AMI

Now you can go back to your `EC2` instance dashboard and save your `AMI` by right clicking and going for `Image->Create Image`.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/AMI-Create-AMI.png" alt="Create AMI">

**Congratulations** you have created your first `AMI`!

Don't forget to terminate your running `EC2` instance from which you created the `AMI` to get prevent any running `EBS` and `EC2` costs.

## Step 2: Creating compute environments and job queues

## Step 3: Creating job definitions

## Step 4: Adjusting resources

## Step 5: Running jobs with AWS Batch
