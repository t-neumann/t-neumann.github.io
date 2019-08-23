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

## Step 1: Estimate resource requirements

Appropriate resource allocation is crucial for setting up AWS workflow that are both cost-efficient and high-throughput. Therefore, I strongly advise you to take a big enough test-dataset, run it on in a limitless test environment - hopefully many of you have some kind of in-house HPC cluster - and take the resulting measurements of resource consumption to find optimal storage, memory and CPU sizes.

Conveniently, Nextflow workflows can be easily executed both on `AWS` but also in your local HPC environment by simply defining additional [profiles](https://www.nextflow.io/docs/latest/config.html#config-profiles) for the scheduler of your choice.

Here is one example of a simple `SLURM` profile:

```java
singularity {
	enabled = true
}

docker {
	enabled = false
}

process {

    executor = 'slurm'
    clusterOptions = '--qos=short'
    cpus = '12'
    memory = { 8.GB * task.attempt }
}

params {

   salmonIndex = '/groups/Software/indices/hg38/salmon/gencode.v28.IMPACT'

}

```

As you can see, usually `HPC` environments do not allow Docker containers to run, but support [Singularity](https://singularity.lbl.gov/) containers which can be [easily built from Docker containers](https://singularity.lbl.gov/docs-build-container#downloading-a-existing-container-from-docker-hub).

The `process` section basically defines the scheduler, resources and the job queue in which the processes should run. Finally, the index files are usually stored in some globally accessible directory, similar to the `s3` storage on `AWS`.

Now that we are set, Nextflow has this neat option flag `-with-report` that gives you a very [comprehensive overview](https://www.nextflow.io/docs/latest/tracing.html#execution-report) of the resources your processes consumed during execution.

Below are the most important excerpts of an example report from when I ran my Nextflow workflow on 1,222 breast cancer datasets from [TCGA](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga):

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/nextflowreport_CPU.png" alt="Nextflow CPU consumption">

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/nextflowreport_memory.png" alt="Nextflow memory consumption">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/nextflowreport_time.png" alt="Nextflow time duration">

On average a single task ran on **6 threads**, consumed **8 GB of memory** and ran **2:30 minutes** - this is the rough framework of resources we will have to consider when allocating resources and choosing appropriate `EC2` instances.

## Step 2: Creating a suitable AMI

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

## Step 3: Creating compute environments and job queues

Now it is time to create appropriate compute environments and their corresponding job queues. I usually like to create some baseline *workload* queue that should handle most of the jobs providing resources estimated from Step 1 and an *excess* queue with very extensive resources that handles the few jobs that overflow the *workload* resources, so that the entire batch is still successfully processed.

### Overview

First, we want to create a new compute environment upon which we can base job queues. For this, go to the `AWS Batch` dashboard -> `Compute Environments`.

I have already created some production environments, for you this overview will probably be empty. Then go to `Create Environment`.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/ComputeEnvironment_Overview.png" alt="Compute environment overview">

### Naming, roles and permissions

First, we want to have a `managed` environment, so `AWS Batch` can do configuration and scaling for us. Now, we can name our compute environment. I chose to create first a `workload` compute environment, thus naming it `salmonWorkload`. Then we simply select the service and instance roles as well as keypairs we created earlier in the `prerequisite` section, there should be only one option to choose from.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/ComputeEnvironment_Names.png" alt="Compute environment naming">

### Some words on instance types and vCPU limits

In my opinion, this part is **the most crucial part** of setting up an optimal environment both in terms of computation and cost efficiency. **So pay special attention here!**

First of all, I hope you did a good enough job in Step 1 of estimating your resource requirements **per task**.

These are the punchlines you have to consider now for fixing instance types and vCPU limits for your compute environment:

* Fit only **1** task in **1** instance!

If you look at the instance pricing table, you will see that prices linearly scaling with instance types - meaning doubling resources results in double prices. You will not save anything by running more jobs on a single larger instance, but you will pay for it since from experience the Docker daemon on the instance sometimes gets confused and hung-up when there's multiple tasks run on the instance.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-architecture/EC2Instances.png" alt="EC2 instances">

* vCPUs refers to the total number of vCPUs of your environments

This got me confused also when trying to figure out, how many instances will be fired up in total. Essentially you have to divide this number by the number of vCPUs provided by your instance type of choice and then you will get the number of instances that will be launched at peak times.

So let's say you chose `c5.2xlarge` as your instance type with 8 vCPUs and your specified `Maximum vCPUs` is 100, then 100 / 8 = 12 instances will be launched in total if the entire compute environment is utilized.

* Keep some spare memory for instance services

I will address this in detail later, but keep in mind that not the entire memory listed in the instance type specification can be used, since some of it will be occupied with running basic instance services.

* Keep homogeneous compute environments

Since we did a careful resource requirement estimation, I find it easiest for keeping track of cost and also ensuring that the tasks will actually finish, to have homogeneous compute environments - meaning one environment will only allow for one specific instance type.

### Specifying instance types and vCPU limits

Now let's put it all together. First up, let's quickly refresh the resource requirements we had per Salmon task:

* We need an instance to provide 8 GB of memory to fit index + data
* If we run our tasks on a 6 thread instance, it will run 2:30 mins

Now if we check the instance type table, we find there is actually 2 types of instances that would cover these requirements:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/ComputeEnvironment_InstanceResearch.png" alt="Potential instance types">

The `c5.xlarge` comes with 8 GB of memory and 4 vCPUs, the `c5.2xlarge` with double the memory and vCPUs. So in principal, we could fit on average on task in the smaller instance, but remember you will have some overhead of services running on the instance that effectively reduces these 8 GB and second these requirements are average requirements, so anything above average will fail to run in such an instance. Therefore, we should definitely go for a `c5.2xlarge` here.

* Choose `c5.2xlarge` as your only instance type and delete `optimal`
* Set `Minimum vCPUs` and `Desired vCPUs` both to 0 to have no idle running instances in background
* Tick the `Enable user-specified Ami ID`, copy the `AMI ID` from the `AMI` we created and validate it

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/AWS-pipeline/ComputeEnvironment_Resources.png" alt="Compute environment resources">

Everything else you can leave empty and click `Create`.

Congratulations, you have created your first compute environment!

### Instance types and vCPU limits

## Step 4: Creating job definitions

## Step 5: Adjusting resources

## Step 6: Running jobs with AWS Batch
